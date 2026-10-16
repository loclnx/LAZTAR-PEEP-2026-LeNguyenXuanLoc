+++
title = "Day 05 - Friday - 16/10/2026 (ON-SITE)"
weight = 5
+++

## Work Completed

- **Prevented duplicate putaway orders**:
  - Hid the create-order button when a putaway order already exists for a receiving slip.
  - Added a server-side check to prevent duplicate order creation.
- **Edited and deleted receiving slips**:
  - Added APIs for editing and deleting receiving slips, along with an edit button in the UI.
  - Allowed these actions only before manager approval, with the server enforcing the restriction.
- **Created receiving slips from the “All Warehouses” filter**:
  - Enabled the create-receiving-slip button.
  - Allowed users to select the destination warehouse in the form.
- **Required a picking completion deadline**:
  - Prevented users from continuing or saving a shipping order until its picking completion deadline is provided.
- **Blocked receiving confirmation before approval**:
  - Required a slip to be assigned and approved by a manager before warehouse receipt can be confirmed.
  - Restricted confirmation to the employee assigned to the slip.
- **Added receiving slips to “Orders for Approval”**:
  - Made unassigned slips visible to managers and admins.
  - Added clear status information and an **Assign & Approve** button.
- **Handled orders with no available items**:
  - Added a **Cancel Entire Order** option, synchronized the order status, and released reserved inventory.
  - Disabled **Deliver Available Items** when no items are available for delivery, and added a server-side restriction as well.

## Summary

The on-site workday focused on completing receiving, approval, and shipping-order handling workflows. The changes add validation on both the UI and server to prevent duplicate or invalid actions, while clarifying assignment and approval steps and handling orders with no available stock.

## Plan for Tomorrow

- Continue testing receiving-slip creation, editing, deletion, and approval workflows.
- Review order-status and inventory consistency after applying out-of-stock resolutions.
