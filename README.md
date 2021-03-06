responses
=========

`responses` is a common library for creating `net/http` responses. Useful for web services using the builtin `/net/http`
library or with Gorilla Mux.

The errors returned by this library uses the JsonError format.

To use:

```
go get -u github.com/darkcrux/mux-responses
```

```
import (
    "encoding/json"
    "net/http"

    "github.com/darkcrux/mux-responses"
)

func handler(res http.ResponseWriter, req *http.Request) {
    body := req.Body
    defer body.Close()
    var data interface{} 
    if err := json.NewDecoder(body).Decode(&data); err != nil {
        responses.WriteUnreadableRequestError(res)
        return
    }
    
    result, err := doSomething(data)
    if err != nil {
        // FOR EXAMPLE ONLY. Avoid returning error 500.
        responses.WriteUnknownError(res)
        return
    }

    responses.WriteOKWithEntity(result)
}
```

