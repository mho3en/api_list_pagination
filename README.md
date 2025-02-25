# api-list-pagination

🎉 A powerful and flexible pagination utility for Django REST Framework (DRF) that simplifies pagination, listing, and exporting data to Excel with ease. Say goodbye to repetitive pagination logic! 🚀

---

## Features ✨

- 🧙 **Flexible Pagination**: Automatically handle paginated responses with customizable page sizes.
- 📜 **Listing Mode**: Retrieve all data without pagination, optionally filtering fields.
- 📊 **Excel Export**: Export data directly to Excel-like responses.
- 🔍 **Distinct Query**: Easily return distinct results for your queries.

---

## Installation 🛠️

1. Install the package from PyPI (coming soon):
   ```bash
   pip install api-list-pagination
   ```

2. Import and use it in your Django REST Framework views.

---

## Quick Start 🚀

### 1. Paginate Your Data

```python
from api_list_pagination import ALP
from rest_framework.views import APIView

class ExampleView(APIView):
    def get(self, request):
        queryset = MyModel.objects.all()
        serializer = MySerializer

        paginator = ALP(request, queryset, serializer)
        return paginator.ret()
```

### 2. Retrieve a Full Listing (No Pagination)

```python
class FullListView(APIView):
    def get(self, request):
        queryset = MyModel.objects.all()
        serializer = MySerializer

        paginator = ALP(request, queryset, serializer)
        response = paginator.listing()
        return response
```

#### Example Request:
```http
GET /example/?limit=-1&fields=name age
```

---

### 3. Export Data to Excel

```python
class ExcelExportView(APIView):
    def get(self, request):
        queryset = MyModel.objects.all()
        serializer = MySerializer

        paginator = ALP(request, queryset, serializer)
        response = paginator.excel()
        return response
```

#### Example Request:
```http
GET /example/?excel=1&fields=name email
```

---

## How It Works ⚙️

`ALP` simplifies common data retrieval scenarios with:
- `pagination()`: Paginated results using DRF's `PageNumberPagination`.
- `listing()`: Non-paginated results with optional field filtering.
- `excel()`: Converts data into Excel-like responses.

It uses the `limit` query parameter to control pagination and supports additional options like:
- `distinct`: Ensure unique results.
- `fields`: Retrieve specific fields from the queryset.
- `values_list`: Output data in list format for single or multiple fields.

---

## API Reference 📖

### ALP(request, data, serializer, context=None)

| Parameter     | Type               | Description                               |
|---------------|--------------------|-------------------------------------------|
| `request`     | `HttpRequest`      | The DRF request object.                   |
| `data`        | `QuerySet`         | The Django queryset to paginate.          |
| `serializer`  | `Serializer`       | The DRF serializer class to use.          |
| `context`     | `dict` (optional)  | Additional serializer context (default: `{}`). |

#### Methods:
- `ret()`: Automatically decide between pagination, listing, or Excel export.
- `pagination()`: Perform pagination on the queryset.
- `listing()`: Retrieve all data without pagination.
- `excel()`: Export data to Excel-like format.

---

## Examples 📚

### Example 1: Paginated Results
Request:
```http
GET /example/?limit=5
```
Response:
```json
{
  "count": 100,
  "next": "http://api.example.com/example/?limit=5&offset=5",
  "previous": null,
  "results": [
    {"id": 1, "name": "Alice"},
    {"id": 2, "name": "Bob"},
    {"id": 3, "name": "Charlie"},
    {"id": 4, "name": "Diana"},
    {"id": 5, "name": "Eve"}
  ]
}
```

### Example 2: Distinct Results
Request:
```http
GET /example/?distinct=1&fields=name
```
Response:
```json
{
  "results": [
    {"name": "Alice"},
    {"name": "Bob"},
    {"name": "Charlie"}
  ]
}
```

### Example 3: Excel Export
Request:
```http
GET /example/?excel=1&fields=name email
```
Response:
```json
[
  {"name": "Alice", "email": "alice@example.com"},
  {"name": "Bob", "email": "bob@example.com"}
]
```

---

## Contributing 🛠️

We welcome contributions! Feel free to fork the repository, submit issues, or create pull requests. Let’s build something awesome together. 💪

---

## License 📄

This project is licensed under the [MIT License](LICENSE).

---

## Support 💬

Have questions or feedback? Feel free to reach out or open an issue on GitHub. We're here to help!

