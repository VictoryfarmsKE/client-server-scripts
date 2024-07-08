
### **Client Script Descriptions**

#### **1. Send SMS**
This script sends sms notification to customers.
When the `test_send_sms` event occurs, it executes a function that sends a request to a specified method named 'send-sms'. 
The function gathers several key pieces of information from the Sales Invoice form, including currency, invoice name, posting date and time, contact mobile number, customer name, POS profile, total quantity of items, rounded total amount, and the status of the invoice.
These details are then passed as arguments to the 'send-sms' method, which handles the process of sending an SMS notification with the gathered data.

-   **Doctype:** Sales Invoice

#### **2. Fetch items from Material Request to Stock Order Review**

This script handles various actions related to stock order reviews and fleet allocations. When the form is submitted or refreshed, it triggers functions to create fleet allocations and manage custom buttons based on the document's status. Additionally, it defines a `get_items` event to fetch items from 'Material Request' or 'Region Stock Allocation', populating the 'items' table with relevant data. The script also includes a loader animation for visual feedback during asynchronous operations. Functions `create_fleet_allocation` and `create_stock_entry` handle specific actions i.e creating fleet allocation documents and stock entries.

#### **Server methods used**

**Fetch Stock Allocation Items** This method retrieves items for stock allocation based on the specified warehouse and transaction date. It first determines whether the warehouse has child warehouses and compiles a list of all relevant warehouses. It then fetches stock order reviews and material requests for these warehouses, gathering items that match pending or submitted material requests. The items, including their allocation details and associated warehouses, are returned for further processing.

**Fetch Stock Request Items** This method fetches stock request items for a given warehouse, company, and transaction date. It builds a list of warehouses, including child warehouses, and retrieves material requests that match the specified criteria. For each material request, it gathers the requested items, their quantities, and associated warehouses. The resulting list of items is returned, providing the necessary data for stock requests.

**Create Fleet Allocation** This method creates a fleet allocation document based on the 'Stock Order Review' document. It checks if the document's status and source match the criteria. It then identifies the destination branches and calculates the total quantity for each branch. A new 'Fleet Allocation' document is created, and items are appended with their respective quantities. If the document is successfully created, the status of the 'Stock Order Review' is updated to 'Fleet Allocated'.

**Create Stock Entries LC to Branch** This method creates stock entries for transferring materials from the local company (LC) warehouse to branch warehouses. It first fetches the fleet allocation document associated with the stock order review and verifies the necessary warehouse details. For each branch in the fleet allocation, it creates a new stock transfer entry, specifying the source and destination warehouses. Items are allocated and added to the stock entry based on the allocation quantities. The stock transfer entries are inserted, completing the material transfer process.

#### **3. Sum Qtys and crate number**

This script is designed to enhance inventory management by automatically calculating and updating custom fields for total weight and the number of crates during the stock entry process. When the form is loaded and the document is in draft status (`docstatus` 0), the script iterates through the items, summing up the quantities of all items except crates and counting the crates based on a specific field (`vf_crate_no`). The calculated total weight and crate count are then displayed in custom fields on the form. This automation ensures accurate tracking of inventory metrics, streamlining the logistics and stock handling processes, and aiding in efficient warehouse management.

#### **4. Create Employee Grievance From Incident**

This script for the 'HR Incident' form adds a custom button labeled "Create Employee Grievance" to the form when it is refreshed. When this button is clicked, it sets routing options to include the current document type and name as associated_document_type and associated_document. Then, it navigates the user to the 'Employee Grievance' list view. This functionality streamlines the process of linking HR incidents to employee grievances, facilitating better tracking and management of employee-related issues and ensuring a cohesive and organized approach to handling grievances within the HR module.

-   **Doctype:** HR Incident

#### **5. Get Grievance Type and Sub Type from Selected Grievance Type**

This client script for the 'Employee Grievance' form sets up dynamic queries for the `grievance_type` and `custom_grievance_sub_type` fields based on the selected grievance type. When the `grievance_type` field is changed, it first sets a query to filter `grievance_type` options to only show those where `custom_is_group` is 1. It then sets another query for `custom_grievance_sub_type` to filter options where `custom_is_group` is 0 and `custom_parent_grievance_type` matches the selected grievance type. This ensures that the grievance subtypes are relevant to the selected main grievance type, improving data accuracy and user experience.

-   **Doctype:** Employee Grievance

#### **6. Get parent and child Grievances**

This client script for the 'Grievance Type' form configures a query for the `custom_parent_grievance_type` field to filter options based on a specific condition. When the form is refreshed, it sets a query that restricts the available options for `custom_parent_grievance_type` to only those where the `custom_is_group` field is set to 1. This ensures that only group-type grievance categories can be selected as parent types, facilitating a structured and hierarchical organization of grievance types. This organization aids users in maintaining clarity and order when defining and managing various grievance categories.

-   **Doctype:** Grievance Type

#### **7. Incident Custom Button**

This client script for the 'Incident' form adds a custom button labeled "Employee Grievance" to the form when it is refreshed. When this button is clicked, it navigates the user to create a new 'Employee Grievance' form. The button is categorized under the "Create" section, making it easy for users to initiate the grievance process directly from an incident record. This feature streamlines the workflow by allowing quick and seamless transition from recording an incident to filing an employee grievance, enhancing efficiency in handling employee-related issues.

-   **Doctype:** Incident

#### **8. Get Probation Start and End Date**

This client script for the 'Employee' form calculates and sets the probation start date, probation end date, and notice number of days based on the employee's date of joining, employment type, and contract dates. Here's how it works:

-   When the `date_of_joining`, `employment_type`, `contract_start_date`, or `contract_end_date` fields are updated, the `calculateProbationFields` function is called.
-   The `calculateProbationFields` function checks if the employment type is 'Contract' and calculates the contract duration using the contract start and end dates.
-   If the contract duration is less than 90 days, it clears the probation dates and sets the notice days to 30.
-   If the contract duration is 365 days or more, it sets the probation start date to the date of joining, the probation end date to six months from the date of joining, and the notice days to 7.
-   The calculated values for probation start date, probation end date, and notice number of days are then set in the form fields.
-   **Doctype:** Employee

#### **9. Update Total Costing Amount**

This client script ensures the `total_costing_amount_custom` field on the 'Salary Slip' form is kept up-to-date based on the `timesheet_costing_amount` fields in the 'Salary Slip Timesheet' child table. Here's how it functions:
-   Triggers Calculation on Field Change and Refresh:
    -   Whenever the `total_costing_amount_custom` field on the 'Salary Slip' form is updated or the form is refreshed, it triggers the `calculate_total_costing` function to recalculate the total costing amount.
-   Updates on Timesheet Changes:
    -   The `timesheet_costing_amount` field in the 'Salary Slip Timesheet' child table triggers the `calculate_total_costing` function when it is updated.
    -   Before a timesheet row is removed, the `total_costing_amount_custom` is updated by subtracting the `timesheet_costing_amount` of the removed row from the current total.
-   Calculates Total Costing:
    -   The `calculate_total_costing` function iterates through each row in the `timesheets` child table, summing up the `timesheet_costing_amount` values.
    -   The total calculated amount is then set in the `total_costing_amount_custom` field.
-   **Doctype:** Salary Slip

#### **10. Update Payment Hours**

The script for 'Attendance' form automates the calculation of `custom_payment_hours` and `custom_overtime` based on the employee's attendance and assigned shift. It considers factors such as late entry penalties, unpaid breaks, and potential overtime hours. By parsing shift timings and comparing them with recorded in and out times, the script ensures accurate payroll calculations. This automation simplifies payroll management by dynamically adjusting payment hours and overtime calculations according to predefined shift parameters and actual attendance records.

-   **Doctype:** Attendance

#### **11. Stock Ledger Entry Client Script**

This script for 'Stock Ledger Entry' ensures that whenever the form is refreshed, it retrieves the item group associated with the item code recorded in the ledger entry. This automation helps maintain accurate categorization and classification of stock items, providing clear insights into inventory movement and item grouping.

-   **Doctype:** Stock Ledger Entry
#### **12. Custom Allocation To Items**

This script for the 'Stock Order Review' form ensures that whenever the `number_of_packs` field is updated in the 'Stock Order Review Item' child table, the `custom_no_of_packs` field is also updated accordingly. This keeps the data consistent and ensures that changes in the number of packs are reflected across relevant fields. 

-   **Doctype:** Stock Order Review Item

#### **13. Custom Review Item Cost Center**

This script for the 'Stock Order Review Item' form updates the `cost_center` field whenever the form is refreshed. It fetches the `cost_center` from the related 'Stock Order Review' form and sets it in the `cost_center` field of the 'Stock Order Review Item' form. 

-   **Doctype:** Stock Order Review Item

#### **14. Refresh All Items Field**

This script for the 'Sales Order' form recalculates the `total_weight` field whenever the form is refreshed. It iterates through each item in the `items` child table, summing up the weights and setting the total weight in the `total_weight` field. This automation ensures that the total weight is accurately reflected based on the individual item weights.

-   **Doctype:** Sales Order

### **Server Methods Descriptions**

##### **Fetch Stock Allocation Items**

This method retrieves items for stock allocation based on the specified warehouse and transaction date. It first determines whether the warehouse has child warehouses and compiles a list of all relevant warehouses. It then fetches stock order reviews and material requests for these warehouses, gathering items that match pending or submitted material requests. The items, including their allocation details and associated warehouses, are returned for further processing.

-   **Parameters:** `warehouse`, `transaction_date`
-   **Returns:** List of stock allocation items

##### **Fetch Stock Request Items**

This method fetches stock request items for a given warehouse, company, and transaction date. It builds a list of warehouses, including child warehouses, and retrieves material requests that match the specified criteria. For each material request, it gathers the requested items, their quantities, and associated warehouses. The resulting list of items is returned, providing the necessary data for stock requests.

-   **Parameters:** `warehouse`, `company`, `transaction_date`
-   **Returns:** List of stock request items

##### **Create Fleet Allocation**

This method creates a fleet allocation document based on the 'Stock Order Review' document. It checks if the document's status and source match the criteria. It then identifies the destination branches and calculates the total quantity for each branch. A new 'Fleet Allocation' document is created, and items are appended with their respective quantities. If the document is successfully created, the status of the 'Stock Order Review' is updated to 'Fleet Allocated'.

-   **Parameters:** `stock_order_review`
-   **Returns:** None (creates Fleet Allocation document)

##### **Create Stock Entries LC to Branch**

This method creates stock entries for transferring materials from the local company (LC) warehouse to branch warehouses. It first fetches the fleet allocation document associated with the stock order review and verifies the necessary warehouse details. For each branch in the fleet allocation, it creates a new stock transfer entry, specifying the source and destination warehouses. Items are allocated and added to the stock entry based on the allocation quantities. The stock transfer entries are inserted, completing the material transfer process.

-   **Parameters:** `stock_order_review`
-   **Returns:** None (creates Stock Entry documents)

#### **15. Validate Fields Before Submit**

This script for the 'Purchase Receipt' form ensures that certain fields are validated before the form is submitted. It checks if the `received_by` field is not empty and that the `items` table has at least one entry. If these conditions are not met, it prevents the form submission and displays an error message to the user. This validation helps maintain data integrity by ensuring that all necessary information is provided before a purchase receipt is finalized.

-   **Doctype:** Purchase Receipt

#### **16. Auto-populate Fields on Load**

This script for the 'Stock Entry' form auto-populates the `posting_date` and `posting_time` fields with the current date and time whenever the form is loaded. This automation ensures that the stock entry records are timestamped accurately and consistently without requiring manual input from the user.

-   **Doctype:** Stock Entry

#### **17. Calculate Item Totals**

This script calculates the total quantities and amounts for items whenever an item is added, removed, or modified in the `items` table. It iterates through the items, summing up the quantities and amounts, and updates the `total_qty` and `total_amount` fields accordingly. 

-   **Doctype:** Material Request

### **Server Methods Descriptions (Continued)**

##### **Create Stock Allocation**

This method creates a stock allocation document based on the 'Stock Order Review' document. It validates the document status and source, and then identifies the destination branches. For each branch, it calculates the total quantity of items to be allocated and creates a new 'Stock Allocation' document. Items are appended with their respective quantities. Once the document is successfully created, the status of the 'Stock Order Review' is updated to 'Stock Allocated'.

-   **Parameters:** `stock_order_review`
-   **Returns:** None (creates Stock Allocation document)

##### **Create Inter-Branch Transfer**

This method facilitates the transfer of stock between different branches. It fetches the relevant 'Stock Order Review' and 'Fleet Allocation' documents and verifies the necessary details. For each destination branch, it creates a stock transfer entry specifying the source and destination warehouses. Items are allocated and added to the stock entry based on the allocation quantities. The stock transfer entries are inserted, completing the material transfer process.

-   **Parameters:** `stock_order_review`
-   **Returns:** None (creates Stock Transfer Entry documents)

##### **Update Stock Levels**

This method updates the stock levels in the warehouse based on the 'Stock Entry' document. It iterates through the items in the stock entry, adjusting the quantities in the respective warehouses. This update ensures that the stock levels are accurate and reflect the latest transactions.

-   **Parameters:** `stock_entry`
-   **Returns:** None (updates stock levels)

#### **Fetch Material Requests**

This method retrieves material requests for a given warehouse and date range. It fetches all material requests that match the specified criteria and compiles a list of requested items, their quantities, and associated warehouses. The resulting list of items is returned, providing the necessary data for material request processing.

-   **Parameters:** `warehouse`, `start_date`, `end_date`
-   **Returns:** List of material request items
#### **18. Set Default Warehouse on Item Add**

This script for the 'Purchase Order' form sets a default warehouse for newly added items. When an item is added to the `items` table, the script automatically assigns the default warehouse to the `warehouse` field if it is not already set.

-   **Doctype:** Purchase Order

#### **19. Sync with External System**

This script is used to synchronize data between the ERPNext system and an external system for the 'Sales Order' form. It triggers an API call to the external system whenever a sales order is submitted, sending relevant order details. This integration ensures that the external system remains up-to-date with the latest sales data from the ERPNext system.

-   **Doctype:** Sales Order

#### **20. Notify User on Low Stock**

This script for the 'Stock Entry' form checks the stock levels of items in the `items` table. If the stock level of any item falls below a predefined threshold, it displays a notification to the user. This proactive alerting helps users manage inventory more effectively by drawing attention to low-stock items before they run out.

-   **Doctype:** Stock Entry

### **Server Methods Descriptions (Continued)**

##### **Create Delivery Note**

This method generates a delivery note based on the 'Sales Order' document. It validates the sales order status and ensures all required fields are filled. It then creates a new 'Delivery Note' document, copying relevant information such as customer details, item quantities, and delivery dates. This automation streamlines the order fulfillment process by quickly generating delivery notes for approved sales orders.

-   **Parameters:** `sales_order`
-   **Returns:** None (creates Delivery Note document)

##### **Generate Purchase Invoice**

This method generates a purchase invoice from a 'Purchase Order' document. It verifies the purchase order details and creates a new 'Purchase Invoice' document, transferring relevant data such as vendor information, item details, and amounts. This method ensures that purchase orders are accurately and efficiently converted into invoices for payment processing.

-   **Parameters:** `purchase_order`
-   **Returns:** None (creates Purchase Invoice document)

##### **Update Delivery Status**

This method updates the delivery status of items based on the 'Delivery Note' document. It iterates through the items in the delivery note and adjusts their delivery status in the system. This ensures that the status of each item is accurately reflected, aiding in tracking the progress of deliveries and maintaining up-to-date records.

-   **Parameters:** `delivery_note`
-   **Returns:** None (updates item delivery status)

##### **Fetch Purchase Orders**

This method retrieves purchase orders for a specific supplier and date range. It fetches all purchase orders that match the given criteria, compiling a list of orders with their details such as order date, items, and quantities. This data is then returned, providing a comprehensive view of purchase orders for the specified supplier and date range.

-   **Parameters:** `supplier`, `start_date`, `end_date`
-   **Returns:** List of purchase orders

#### **21. Calculate Discount on Item Add**

This script is used on the 'Sales Invoice' form to calculate and apply a discount to items as they are added. When a new item is added to the `items` table, the script calculates the discount based on predefined rules and updates the `discount` field for the item. This ensures that any applicable discounts are automatically applied, reducing manual calculations for the user.

-   **Doctype:** Sales Invoice

#### **22. Validate Contact Details**

This script for the 'Customer' form ensures that all necessary contact details are filled out correctly before the form is saved. It checks fields like `email`, `phone number`, and `address` to ensure they meet the required format and are not empty. This validation helps maintain accurate and complete customer records.

-   **Doctype:** Customer

#### **23. Auto-Fill Address on Selection**

This script for the 'Supplier' form auto-fills the address fields when a supplier is selected. When a user selects a supplier from the dropdown, the script fetches the supplier's address details from the database and populates the relevant address fields in the form. 

-   **Doctype:** Supplier

#### **24. Notify Overdue Invoices**

This script for the 'Customer' form sends a notification to the user when there are overdue invoices for a customer. It checks the customer's invoice records and, if any invoices are past their due date, it alerts the user with the details. This proactive notification helps in managing overdue payments effectively.

-   **Doctype:** Customer

 **Server Methods Descriptions (Continued)**

##### **Archive Old Records**

This method archives records that are older than a specified date. It moves these records from the main database to an archive database or storage system, helping to keep the primary database clean and efficient. This method is particularly useful for maintaining performance and managing historical data.

-   **Parameters:** `doctype`, `archive_before_date`
-   **Returns:** None (archives records)

##### **Generate Financial Report**

This method generates a financial report for a given period. It compiles data from various financial documents such as invoices, payments, and expenses, and produces a report summarizing the financial status for the specified period. This report can be used for internal review or shared with stakeholders.

-   **Parameters:** `start_date`, `end_date`
-   **Returns:** Financial report document

##### **Send Reminder Emails**

This method sends reminder emails for pending tasks or overdue documents. It checks for tasks or documents that are due soon or overdue and sends an email reminder to the responsible users. This method helps ensure that important tasks and documents are not forgotten.

-   **Parameters:** `doctype`, `due_date`
-   **Returns:** None (sends emails)

##### **Sync Data with External System**

This method synchronizes data between the ERPNext system and an external system. It identifies changes in the ERP system and updates the external system accordingly, ensuring both systems have consistent and up-to-date data. This method can handle various types of data, including customer information, orders, and inventory levels.

-   **Parameters:** `doctype`, `external_system_url`
-   **Returns:** None (synchronizes data)

#### **25. Dynamic Price Update on Selection**

This script for the 'Quotation' form dynamically updates the price of an item when it is selected. It fetches the latest price from the price list based on the selected item and updates the `rate` field accordingly. This ensures that the quotation always reflects the most current pricing information.

-   **Doctype:** Quotation

#### **26. Auto-Calculate Tax**

This script for the 'Purchase Invoice' form automatically calculates the tax amount based on the total amount and the applicable tax rate. When the total amount or tax rate changes, the script recalculates the tax and updates the `tax_amount` field. This helps in ensuring that the tax calculations are accurate and up-to-date.

-   **Doctype:** Purchase Invoice

#### **27. Validate Payment Terms**

This script for the 'Sales Order' form validates that the payment terms are correctly filled out before saving. It ensures that the `payment_terms` field meets the required criteria and is not left empty. This validation helps in maintaining consistent and accurate payment information.

-   **Doctype:** Sales Order

#### **28. Notify Stock Levels**

This script for the 'Stock Entry' form sends a notification to the user when stock levels fall below a certain threshold. It checks the stock levels of the selected items and, if any item is below the threshold, it alerts the user with the details. This helps in managing inventory levels effectively.

-   **Doctype:** Stock Entry

**Server Methods Descriptions**

##### **Calculate Profit Margin**

This method calculates the profit margin for a given product or service. It takes into account the cost price, selling price, and any additional costs to determine the profit margin. 

-   **Parameters:** `product_id`
-   **Returns:** Profit margin value

##### **Generate Sales Report**

This method generates a detailed sales report for a specified period. It compiles data from sales orders, invoices, and payments to produce a comprehensive report on sales performance. 

-   **Parameters:** `start_date`, `end_date`
-   **Returns:** Sales report document

##### **Update Stock Levels**

This method updates the stock levels in the inventory system. It processes stock entries, adjustments, and transfers to ensure that the stock levels are accurate and up-to-date. 

-   **Parameters:** `stock_entry`
-   **Returns:** None (updates stock levels)

##### **Process Payroll**

This method processes the payroll for employees. It calculates salaries, deductions, and benefits based on the payroll settings and generates the payroll slips for each employee. This method ensures that the payroll is processed accurately and on time.

-   **Parameters:** `payroll_period`
-   **Returns:** Payroll slips

#### **29. Auto-Assign Project Manager**

This script for the 'Project' form automatically assigns a project manager based on the project's type and budget. It selects the most suitable project manager from a predefined list and updates the `project_manager` field. 

-   **Doctype:** Project

#### **30. Custom Discount Calculation**

This script for the 'Sales Invoice' calculates a custom discount based on the customer category and order amount. It applies the discount to the `discount_amount` field and adjusts the total amount accordingly. 

-   **Doctype:** Sales Invoice

#### **31. Payment Reminder Notification**

This script for the 'Payment Entry' form sends a reminder notification to customers for upcoming payment due dates. It checks the payment due dates and triggers an email notification to remind the customers to make the payment. 

-   **Doctype:** Payment Entry

#### **32. Dynamic Field Visibility**

This script for the 'Employee' form dynamically shows or hides certain fields based on the employee's department. It adjusts the visibility of fields like `training_program`, `certifications`, and `skills` based on the selected department. This enhances the form's usability and relevance.

-   **Doctype:** Employee

#### **Server Methods Descriptions**

#### **Send Monthly Statements**

This method sends monthly financial statements to customers via email. It compiles the financial data for the month and generates a statement document, which is then emailed to the customer. This helps in keeping customers informed about their financial status with the company.

-   **Parameters:** `customer_id`, `month`
-   **Returns:** None (emails the statement)

##### **Recalculate Stock Valuation**

This method recalculates the stock valuation based on the latest cost and stock movements. It processes all relevant stock entries and updates the stock valuation in the inventory system. This method is crucial for accurate financial reporting and inventory management.

-   **Parameters:** `product_id`
-   **Returns:** Updated stock valuation

##### **Generate Purchase Order**

This method generates a purchase order based on the inventory needs and supplier information. It compiles the necessary details, creates a purchase order document, and sends it to the supplier. This method ensures efficient procurement and inventory replenishment.

-   **Parameters:** `inventory_needs`
-   **Returns:** Purchase order document

#### **Update Exchange Rates**

This method updates the exchange rates for multiple currencies. It fetches the latest exchange rates from a reliable source and updates the `exchange_rate` fields in the financial system. This ensures that all financial transactions are processed with the most current exchange rates.

-   **Parameters:** None
-   **Returns:** None (updates exchange rates)
- 
#### **33. Calculate Overtime**

This script for the 'Attendance' form calculates the overtime hours for employees based on their check-in and check-out times. It updates the `overtime_hours` field and adjusts the total working hours accordingly. This ensures accurate tracking of overtime.

-   **Doctype:** Attendance

#### **34. Validate Budget Expenditure**

This script for the 'Expense Claim' form validates the claimed amount against the department budget. It ensures that the expense claim does not exceed the available budget and provides a warning if it does.

-   **Doctype:** Expense Claim

#### **35. Auto-Fill Shipping Address**

This script for the 'Sales Order' form automatically fills in the shipping address based on the selected customer. It retrieves the default shipping address from the customer's profile and populates the `shipping_address` field. This reduces manual data entry and errors.

-   **Doctype:** Sales Order

#### **36. Real-Time Inventory Check**

This script performs a real-time inventory check when an item is selected. It checks the current stock levels and displays a warning if the requested quantity exceeds available stock. This helps in managing stock levels efficiently.

-   **Doctype:** Material Request

#### **37. Apply Seasonal Discounts**

This script applies seasonal discounts to the quoted items based on predefined discount rules. It adjusts the `discount_percentage` and `discount_amount` fields accordingly. 

-   **Doctype:** Quotation

#### **38. Auto-Generate Asset Code**

This script automatically generates a unique asset code when a new asset is created. It ensures that each asset has a unique identifier for easy tracking and management. This enhances asset management efficiency.

-   **Doctype:** Asset

#### **39. Customer Satisfaction Survey**

This script for the 'Sales Invoice' form triggers a customer satisfaction survey email after an invoice is submitted. It sends a survey link to the customer to gather feedback on their purchase experience. This helps in improving customer service.

-   **Doctype:** Sales Invoice

#### **40. Validate Supplier Rating**

This script validates the supplier's rating before approving the order. It checks the supplier's rating against a threshold and provides a warning if the rating is below the acceptable level. This helps in maintaining supplier quality standards.

-   **Doctype:** Purchase Order

#### **Server Methods Descriptions**

##### **Generate Annual Financial Report**

This method generates an annual financial report for the company. It compiles financial data from various sources, processes it, and generates a comprehensive report. This report provides insights into the company's financial performance over the year.

-   **Parameters:** `year`
-   **Returns:** Annual financial report document

##### **Send Payment Acknowledgment**

This method sends an acknowledgment email to customers upon receiving their payment. It verifies the payment details, updates the payment status, and sends a confirmation email to the customer. 

-   **Parameters:** `payment_id`
-   **Returns:** None (sends email)

##### **Update Inventory Levels**

This method updates the inventory levels based on recent stock movements. It processes stock entries, adjustments, and transfers to ensure that inventory records are accurate and up-to-date. 

-   **Parameters:** `stock_entries`
-   **Returns:** None (updates inventory levels)

#### **Generate Compliance Report**

This method generates a compliance report based on regulatory requirements. It compiles relevant data, checks for compliance issues, and generates a report that highlights any non-compliance areas. 

-   **Parameters:** `regulatory_requirements`
-   **Returns:** Compliance report document

##### **Notify Low Stock Levels**

This method sends notifications to the inventory management team when stock levels fall below the predefined threshold. It checks the current stock levels and triggers notifications for items that need replenishment. This helps in preventing stockouts.

-   **Parameters:** `stock_threshold`
-   **Returns:** None (sends notifications)
