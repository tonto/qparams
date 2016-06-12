# qparams
### `import “github.com/tonto/qparams”`
qparams is a golang package that provides url query parameters to be decoded to user defined golang structs with custom fields.

# Example usage
```
import qp “github.com/tonto/qparams”

// User defined struct
type MyParams struct {
	// qparams Map (map[string]string)
	// with defined operations (required)
	Filter qp.Map `qparams:”ops:>=,==,!=“`

	// qparams Slice ([]string)
	Embed qp.Slice 

	// qparams Slice ([]string)
	// with custom separator
	Flags qp.Slice `qparams:”sep:|”`

    // field with custom query param name (lowercase is the default)
    FooBar string `qparams:"name:foo-bar"`

	// Regular primitive values
	Limit int
	Page int
}

…

// Your http handler
func handler(w http.ResponseWriter, r *http.Request) {
	params := MyParams{}

	err := qp.Parse(&params, r)
	if err != nil {
		// Handle error
	}

	// at this point params is filled with 
	// parsed query parameters
	//

	// Assuming that request URL was:
	// http://foo.bar.baz?limit=100&page=2&embed=order,invoice,discount&filter=amount>=1000,currency==EUR,order_number!=753&flags=a|b|c
	//
	// params would contain following values:
	//
	// params.Filter 
	// qp.Map{
	//   “amount >=“: “1000”,
	//   “currency ==“: “EUR”,
	//   “order_number !=“: “753”,
	// }
	// params.Filter.Map() will convert it to map[string]string
	//
	// params.Embed
	// qp.Slice{“order”, “invoice”, “discount”}
	// params.Embed.Slice() will convert it to []string
	//
	// params.Flags
	// qp.Slice{“a”, “b”, “c”}
	// params.Embed.Slice() will convert it to []string
	//
	// params.Limit
	// int 100
	//
	// params.Page
	// int 2
}
```	

# Features
# Docs
[godoc.org/github.com/tonto/qparams](http://godoc.org/github.com/tonto/qparams)