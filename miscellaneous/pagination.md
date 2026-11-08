# Pagination
Shows one page at a time from a container of pages, with previous / next buttons and a page label (for example "2 / 5").

## Setup
1. Create a **Pages Container** and put each page as a child object.
2. Add **Pagination** and assign the **Pages Container**.
3. Assign the **Button Prev** and **Button Next**.
4. Optionally assign a **Label Text** and the **Label Active Color** used for the current page number.

Only the active page's GameObject is enabled. Paging wraps around from the last page to the first.
