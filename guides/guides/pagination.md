# Paginated Results Guide

DeployForge uses paginated responses to efficiently handle large datasets. This guide explains how to work with paginated API responses when interacting with deployment histories, resource listings, and audit logs.

## Understanding Pagination Parameters

Our API endpoints support three query parameters for result pagination:

1. **`page`** (integer):  
   The page number to retrieve (default: `1`)
   
2. **`page_size`** (integer):  
   Number of items per page (default: `50`, max: `500`)
   
3. **`max_items`** (integer):  
   Maximum total items to return across all pages (use for result limiting)

## Sample Request
```bash
curl -X GET "https://api.deployforge.com/v1/deployments?page=2&page_size=100" \
  -H "Authorization: Bearer $DEPLOYFORGE_TOKEN"
```

## Response Structure
Paginated responses include pagination metadata in the `pagination` object:

```json
{
  "data": [...],
  "pagination": {
    "page": 2,
    "page_size": 100,
    "total_items": 427,
    "total_pages": 5,
    "has_next": true,
    "next_page": 3
  }
}
```

## Processing Paginated Results
1. **First Request**:  
   Start with initial request (no page parameters)
   
2. **Subsequent Requests**:  
   Check pagination properties and request next page when available:
   ```javascript
   while (response.pagination.has_next) {
     // Fetch next page
     response = await fetchResults(response.pagination.next_page);
     // Process data...
   }
   ```

## Best Practices
- **Default Behavior**: Omit parameters to use default pagination
- **Batch Processing**: Use larger `page_size` (≤500) for bulk operations
- **Rate Limiting**: Add 100-500ms delays between page requests
- **Result Limiting**: Use `max_items` when you need partial results
- **Error Handling**: Implement retries for `429 Too Many Requests` responses

## Troubleshooting
- **Empty Results**: Verify `page` parameter isn't higher than `total_pages`
- **Partial Results**: Check if `max_items` parameter is limiting returns
- **Performance Issues**: Reduce `page_size` if experiencing timeouts

For complete response examples and field definitions, reference our [API Documentation](/api-reference).