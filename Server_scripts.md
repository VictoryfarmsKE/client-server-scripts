## Server Scripts

**1. Get Variance on End Transit**

This script automates the creation of Material Transfer - Variance Stock Entries based on outgoing stock entries associated with a Material Transfer document (doc). It identifies discrepancies in transferred quantities for items, excluding specified codes, and generates corresponding entries to accurately reflect inventory movements and variances between expected and actual quantities.

**2. Job Opening Closure Automation**

This script automates the rejection of job applicants for job openings that have been marked with a custom flag (custom_reject_unoffered_applicants). It retrieves all closed job openings and iterates through associated job applicants. If an applicant's status is not "Accepted", it updates their status to "Rejected" in the Job Applicant record.

**3. Update Grievance Status Scheduler**

This scheduler script automates the update of Employee Grievance statuses for grievances related to "Stock loss." It retrieves all Employee Grievance records with the specified subject and checks if any custom actions within these records have expired based on a predefined logic (e.g., 12 months from the date issued). If an action's expiry date has passed, its status is set to "Inactive."

**4. Fetch Total Costing Amount**

This script updates the timesheet_costing_amount field for each timesheet linked to the doctype. It retrieves the total costing amount from each referenced timesheet and sets this value in the corresponding timesheet_costing_amount field. This ensures accurate tracking and reporting of costing data associated with each timesheet entry, enhancing financial transparency and operational oversight.

**5. Set SKU Summary for Stock Order Review**

This server script automates the calculation and aggregation of allocated quantities (`allocated_qty`) for each item (`item_code`) in a Stock Order Review. When the doctype's status is draft (`doc.docstatus == 0`), it iterates through the items listed, accumulating their allocated quantities into a summary (`custom_sku_summary`). This summary provides a consolidated view of item allocations.

**6. POS Close Session at 23:59**

This server script automates the closure of open POS sessions and updates their expected amounts and closing balances daily at 23:59. It queries the POS Invoice table for invoices that are open and were created on the current day. For each qualifying invoice, it calculates the expected amount by subtracting the change amount from the paid amount. It then updates the invoice's status to 'Closed' and sets the expected amount and closing balance accordingly. Finally, it commits the changes to the database and notifies the user of successful closure and updates.

**7. Set SKU Summary**

This server script generates a summary of SKU quantities (`custom_sku_summary`) for a Stock Entry doctype when its status is draft (`doc.docstatus == 0`). It initializes an empty dictionary sku_summary to aggregate quantities of items by their item codes. It iterates through each item in the Stock Entry doctype, updating the sku_summary with the cumulative quantity per item code. After processing all items, it sets the `custom_sku_summary `field in the doctype to an empty list and populates it by appending each item code and its respective aggregated quantity. This script provides a consolidated view of item quantities before the Stock Entry doctype is finalized.

**8. Add Sales Persons to Sales Invoice**

This script automates the assignment of sales team members in Sales Invoices based on associated POS profiles. When a Sales Invoice is in draft status and a POS profile is specified, the script retrieves sales persons linked to the POS profile from the database. It captures their names and their allocated percentage. Subsequently, it updates the Sales Invoice's sales_team field, appending each salesperson along with their allocated percentage.

**9. Cancel Variance Stock Entry**

This script facilitates the cancellation of associated variance stock entries in the Stock Entry doctype. It specifically targets stock entries that have the purpose set as "Material Transfer" and have an outgoing stock entry linked. For each of these entries, it queries and retrieves all related stock entries that are already submitted and share the same source variance stock entry (`source_variance_stock_entry`). Subsequently, it sets their document status to cancelled and saves these changes, effectively cancelling the variance stock entries.

**10. Link Lead Mobile Number to Customer and Update Lead's Status**

This script checks if a mobile number is provided. If so, it constructs a SQL query to search for leads that match this mobile number and have statuses including "Lead," "Open," "Replied," "Opportunity," "Quotation," or "Interested." The query ensures that only relevant leads are considered for potential conversion. Upon finding a matching lead, it updates its status to "Converted" and assigns the lead's name to the current document (doc.lead_name). This automation streamlines lead management by automatically progressing leads with specified mobile numbers through the conversion process.

**11. Create Corresponding Crate No for New Serial No**

This script checks if the item_code matches specific types ("Crate", "Rotogal 600", "Rotogal 1400", "Rotogal 70"). If so, it verifies whether a corresponding VF Crate Number (`vf_crate_no`) exists in the database for the provided serial_no. If not found, it creates a new VF Crate Number entry (VF Crate No) associating the serial_no, item_code, and company from the document. This automation ensures that VF Crate Numbers are systematically managed and recorded for relevant items.

**12. Delete Integration Request on Payment Request Delete**

This script retrieves integration requests (Integration Request) associated with a specific Payment Request doctype. If any integration requests are found, it deletes them from the system. Integration requests typically facilitate automated data exchange or actions between ERP systems and external services, ensuring seamless transaction processing and data synchronization.

**13. Update Allow Valuation to Zero**

This snippet checks if the purpose of the document is "Material Transfer" and then iterates through each item in the document. If an item's allow_zero_valuation_rate is 0, it sets it to 1. This action might be part of a process where zero valuation rates need to be allowed under certain conditions, ensuring accurate inventory valuation during material transfers.

**14. Update Destination Warehouse on Material Request**

This script ensures that when creating a material transfer request (Material Request) where the type is specified as "Material Transfer", each item's destination warehouse (destination_warehouse) defaults to its current warehouse (warehouse) if not explicitly set.

**15. Validate Duplicate VF Crate No in Stock Entry**

This script iterates through the items in a document to collect VF crate numbers (`vf_crate_no`) into a list. If any VF crate numbers are found, it then creates a dictionary (vf_crate_dict) to count occurrences of each VF crate number. If any VF crate number appears more than once, it raises an error indicating a duplicate VF crate number.

**16. Update Stock Order Review Status After Submit**

This script effectively changes the status of the specified document to indicate that it has been submitted.

**17. Update Destination Warehouse for Transfer to LC or Branch SC**

This code snippet checks if the doc object has a destination_warehouse field set. If it does, it iterates through each item (d) in the document's items list. For each item where `d.destination_warehouse` is None, it sets `d.destination_warehouse` to match the `doc.destination_warehouse` value.

**18. Reject Job Offers Past Due Date**

This script runs as a scheduler event on an hourly basis. It retrieves all job offers that are currently in a status of "Awaiting Response" (status) and whose valid_till date (valid_till) is before the current date and time (current_date). For each job offer that meets these criteria, the script changes its status to "Rejected" and attempts to save the document. 

**19. Scheduler to Close Old Similar Item Price on New One**

This scheduler event runs hourly to manage item pricing transitions within ERPNext. It identifies new item prices that are set to become effective on the current day and are approved for selling (`selling = 1`). The script then identifies existing item prices matching the attributes of these new prices and scheduled to end just before their validity starts. It updates these existing prices by setting their valid_upto field to one day before the valid_from date of the new price, ensuring seamless pricing transitions and maintaining accuracy in pricing data.

**20. Set Last Maintenance Alert Date on Asset**

This daily scheduler event script retrieves assets flagged for maintenance (`maintenance_required = 1`) and whose last maintenance date (`last_maintenance_date`) is exactly 91 days ago from the current date. For each qualifying asset, it adjusts the last_maintenance_date by moving it back one day and resetting it to the previous day's date. This automation ensures timely alerts for maintenance actions on assets, enhancing operational efficiency and asset management.

**21. Update Status for Overdue Documents**

This script runs as a scheduler event, checking for documents (e.g., invoices, purchase orders) that are overdue based on their due date. It updates the status of these documents to 'Overdue' and triggers any necessary notifications or follow-up actions to ensure the timely handling of overdue items.

**22. Synchronize Customer Data**

This server script synchronizes customer data between ERPNext and an external CRM system. It periodically fetches updated customer records from the CRM, compares them with existing records in ERPNext, and updates or creates new customer records accordingly. This ensures data consistency across systems and facilitates seamless customer management.

**23. Auto-Approve Purchase Orders**

This script automates the approval of purchase orders that meet certain criteria (e.g., total amount below a threshold, specific suppliers). It runs as a scheduler event, checking for pending purchase orders and approving those that qualify. 

**25. Notify Low Stock Levels**

This server script monitors inventory levels and sends notifications when stock levels fall below a predefined threshold. It runs periodically, checking each item in the inventory and triggering alerts for low-stock items. This helps in maintaining optimal stock levels and preventing stockouts.

**26. Update Employee Leave Balance**

This script updates the leave balance of employees at the end of each month. It calculates the leave accrued, deducts any leave taken, and updates the employee's leave balance accordingly. This ensures accurate tracking of leave entitlements and usage.

**27. Validate Supplier Invoice Details**

This script validates the details of supplier invoices before they are submitted. It checks for discrepancies in invoice amounts, missing fields, and other validation rules. If any issues are found, it raises errors to prevent submission until the issues are resolved.

**28. Calculate Project Costing**

This script calculates the total costing for projects based on timesheets, expenses, and other related documents. It aggregates the costs, updates the project costing fields, and provides a detailed breakdown of costs associated with each project. 

**29. Update Employee Performance Records**

This script updates the performance records of employees based on periodic evaluations. It fetches evaluation results, updates performance metrics, and generates performance reports. 

**30. Synchronize Product Data**

This server script synchronizes product data between ERPNext and an external product management system. It periodically fetches updated product information, compares it with existing records in ERPNext, and updates or creates new product records accordingly. This ensures data consistency and up-to-date product information across systems.

**31. Fetch Stock Allocation Items**  
This method retrieves items for stock allocation based on the specified warehouse and `transaction date`. It first determines whether the `warehouse` has child warehouses and compiles a list of all relevant warehouses. It then fetches stock order reviews and material requests for these warehouses, gathering items that match pending or submitted material requests. The items, including their allocation details and associated warehouses, are returned for further processing.

**32. Fetch Stock Request Items**  
This method fetches stock request items for a given warehouse, company, and transaction date. It builds a list of warehouses, including child warehouses, and retrieves material requests that match the specified criteria. For each material request, it gathers the requested items, their quantities, and associated warehouses. The resulting list of items is returned, providing the necessary data for stock requests.

**33. Create Fleet Allocation**  
This method creates a fleet allocation document based on the 'Stock Order Review' document. It checks if the document's status and source match the criteria. It then identifies the destination branches and calculates the total quantity for each branch. A new 'Fleet Allocation' document is created, and items are appended with their respective quantities. If the document is successfully created, the status of the 'Stock Order Review' is updated to 'Fleet Allocated'.

**34. Create Stock Entries LC to Branch**  
This method creates stock entries for transferring materials from the local company (LC) warehouse to branch warehouses. It first fetches the fleet allocation document associated with the stock order review and verifies the necessary warehouse details. For each branch in the fleet allocation, it creates a new stock transfer entry, specifying the source and destination warehouses. Items are allocated and added to the stock entry based on the allocation quantities. The stock transfer entries are inserted, completing the material transfer process.

**35. Create Stock Allocation**  
This method creates a stock allocation document based on the 'Stock Order Review' doctype. It validates the document status and source and then identifies the destination branches. For each branch, it calculates the total quantity of items to be allocated and creates a new 'Stock Allocation' document. Items are appended with their respective quantities. Once the document is successfully created, the status of the 'Stock Order Review' is updated to 'Stock Allocated'.

**36. Create Inter-Branch Transfer**  
This method facilitates the transfer of stock between different branches. It fetches the relevant 'Stock Order Review' and 'Fleet Allocation' doctype and verifies the necessary details. For each destination branch, it creates a stock transfer entry specifying the source and destination warehouses. Items are allocated and added to the stock entry based on the allocation quantities. The stock transfer entries are inserted, completing the material transfer process.

**37. Update Stock Levels**  
This method updates the stock levels in the warehouse based on the 'Stock Entry' document. It iterates through the items in the stock entry, adjusting the quantities in the respective warehouses. This update ensures that the stock levels are accurate and reflect the latest transactions.

