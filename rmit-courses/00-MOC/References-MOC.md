# References — Map of Content

Raw-intake catalog: one record per attachment received (chat or `99-Inbox`), logged
the moment it arrives, independent of whether it's fully processed yet.

```dataview
TABLE course, week, type, status, drive_link
FROM "second-brain/rmit-courses/07-References"
WHERE file.name != "_template"
SORT course ASC, date_received DESC
```
