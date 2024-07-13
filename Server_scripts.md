## Server Scripts

**1. Get Variance on End Transit**

This script automates the creation of Material Transfer - Variance Stock Entries based on outgoing stock entries associated with a Material Transfer document (doc). It identifies discrepancies in transferred quantities for items, excluding specified codes, and generates corresponding entries to accurately reflect inventory movements and variances between expected and actual quantities.

**2. Job Opening Closure Automation**

This script automates the rejection of job applicants for job openings that have been marked with a custom flag (custom_reject_unoffered_applicants). It retrieves all closed job openings and iterates through associated job applicants. If an applicant's status is not "Accepted", it updates their status to "Rejected" in the Job Applicant record.

**3. Update Grievance Status**

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

This script checks if the item_code matches specific types ("Crate", "Rotogal 600", "Rotogal 1400", "Rotogal 70"). If so, it verifies whether a corresponding VF Crate Number (`vf_crate_no`) exists in the database for the provided serial_no. If not found, it creates a new VF Crate Numbr entry (VF Crate No) associating the serial_no, item_code, and company from the document. This automation ensures that VF Crate Numbers are systematically managed and recorded for relevant items.

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

**23. Workflow state change - PO**

This script automates the approval of purchase orders that meet certain criteria (e.g., total amount below a threshold, specific suppliers). It runs as a scheduler event, checking for pending purchase orders and approving those that qualify. 

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

**36. Fetch Uom**
The code retrieves the unit of measure (UOM) for a given item code from request. It first fetches the item code from the request and attempts to get the 'material_request_uom' from the "Item" table. If found, it retrieves the conversion factor from the "UOM Conversion Detail" table or defaults to 1 if not available, then sets the response message with the UOM and conversion factor. If no 'material_request_uom' is found, it sets the response message with the 'stock_uom' instead. If an error occurs, it logs the error, shows a message to the user, and sets the response message to `None`.

**37 Get employee Id of currently logged in user**

The code retrieves the employee ID of the currently logged-in user from the "Employee" table. It then fetches and returns a list of employees who report to this user, or an empty list if none are found, while handling any errors that may occur.

**38 Get Actual Qty on Stock Entry Get Item **

The code retrieves the total actual quantity of a specified item in a specified warehouse from the "tabBin" table. It constructs a query with the item code and warehouse, executes the query to get the sum of the actual quantity, and sets the result as the response message.

**39. Get branch target **

The code retrieves the most recent entry for a specified warehouse from the "Branch Target Item" table, filtered by a confirmed document status (submitted). It fetches the target quantity, target percent allocation, and parent fields, orders the results by creation date in descending order, and limits the result to the latest entry. 

**40. Get actual qty for stock order review item **

The code retrieves the total actual quantity of a specified item in a specified warehouse from the "tabBin" table. It executes a query to sum the actual quantity for the given item and warehouse. If a result is found, it rounds the quantity divided by 25; if no result is found, it sets the result to `None`. The final response message is set to the result as a float, defaulting to 0.0 if no quantity is found or if an error occurs, with the error being logged and a message displayed.

**41. Get active serial numbers in warehouse **

The code retrieves all active serial numbers from a specified warehouse and company from the "Serial No" table. It filters the serial numbers by the given warehouse, company, and status ('Active'), and returns the list of serial numbers. 

**42. Fetch items from item_group**
The code fetches all items from a specified item group, including its subgroups if it has any. It first checks if the given item group has children. If it does, it retrieves all subgroups and repeats the process until all subgroups without children are identified. Then, it fetches items from these final item groups that are not disabled and includes their item codes, material request UOM, and stock UOM. The result is set as the response message.
'
**43. Get Stock Entry File Attachment **

The code retrieves file attachments associated with a specific Stock Entry document. It gets the document name and doctype from the form data and fetches all files attached to this document. If there are any attachments, it sets the response message to `True`.

**44. Get Stock Ledger for Transfer **

The code queries the Stock Ledger Entries (SLE) to fetch data for transferring items from one warehouse to another. It specifies criteria such as the warehouse, company, posting date, item code conditions, and ensures that the quantities are positive and transactions are not cancelled. If matching entries are found, it retrieves details like company, voucher type, voucher number, warehouse, item code, quantities, valuation rate, and other relevant information. If data is retrieved successfully, it sets the response message with the queried entries.

**45. Get target warehouses **

This code retrieves a list of warehouses based on a specified region. It starts by fetching the parent warehouse for the given region. Then, it checks if the warehouse has children; if it does, it recursively fetches all sub-warehouses under the parent warehouse, ensuring they are not disabled. The final list includes warehouses without children, along with their parent warehouse information. The response message is set to this final list of warehouses.

**46. Get VF Crate No Actual Qty **

This code retrieves the sum of actual quantities (`actual_qty`) for a specific VF crate number (`vf_crate_no`) across all warehouses belonging to a given company. It queries the "Stock Ledger Entry" table, filtering by the provided crate number and company, ensuring entries are not cancelled (`is_cancelled = 0`), and groups results by the crate number. If matching entries are found, it sets the response message with the sum of quantities grouped by the crate number.


