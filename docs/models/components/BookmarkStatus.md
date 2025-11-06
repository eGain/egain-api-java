# BookmarkStatus

Article Bookmark Status


## Fields

| Field                                                                      | Type                                                                       | Required                                                                   | Description                                                                |
| -------------------------------------------------------------------------- | -------------------------------------------------------------------------- | -------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| `isBookmarked`                                                             | *boolean*                                                                  | :heavy_check_mark:                                                         | Indicates whether the Article is bookmarked                                |
| `bookmarkID`                                                               | *Optional\<Long>*                                                          | :heavy_minus_sign:                                                         | The ID of the Bookmark, if Article is bookmarked.                          |
| `folderBreadcrumb`                                                         | [Optional\<FolderBreadcrumb>](../../models/components/FolderBreadcrumb.md) | :heavy_minus_sign:                                                         | This schema contains one or more FolderSummary instances.                  |