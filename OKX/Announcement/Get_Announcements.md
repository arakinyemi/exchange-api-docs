### Get announcements

- Response sorted by `pTime` with the most recent first.
- Sorting is not affected by updates to announcements.
- 20 records per page.
- Authentication optional. Responses filtered based on request IP if public, or country if private.
- Rate limit: 5 requests per 2 seconds (User ID for private, IP for public).
- Permission: Read

HTTP Request

`GET /api/v5/support/announcements`

Request Parameters

| Parameter | Type   | Required | Description                           |
|-----------|--------|----------|---------------------------------------|
| annType   | String | No       | Announcement type. See "Announcement types" endpoint. Returns all if not provided. |
| page      | String | No       | Page number for pagination. Default: 1 |

Response Parameters

| Parameter  | Type   | Description                                  |
|------------|--------|----------------------------------------------|
| totalPage  | String | Total number of pages                         |
| details    | Array  | List of announcements                         |
| > title    | String | Announcement title                            |
| > annType  | String | Announcement type                             |
| > pTime    | String | Publish time, Unix timestamp in milliseconds |
| > url      | String | Announcement URL                              |

---
