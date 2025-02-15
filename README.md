# LJSON

ljson Go is a library that parse loose json string

---

[![Go Reference](https://pkg.go.dev/badge/github.com/bububa/ljson.svg)](https://pkg.go.dev/github.com/bububa/ljson)
[![Go](https://github.com/bububa/ljson/actions/workflows/go.yml/badge.svg)](https://github.com/bububa/ljson/actions/workflows/go.yml)
[![goreleaser](https://github.com/bububa/ljson/actions/workflows/goreleaser.yml/badge.svg)](https://github.com/bububa/ljson/actions/workflows/goreleaser.yml)
[![GitHub go.mod Go version of a Go module](https://img.shields.io/github/go-mod/go-version/bububa/ljson.svg)](https://github.com/bububa/ljson)
[![GoReportCard](https://goreportcard.com/badge/github.com/bububa/ljson)](https://goreportcard.com/report/github.com/bububa/ljson)
[![GitHub license](https://img.shields.io/github/license/bububa/ljson.svg)](https://github.com/bububa/ljson/blob/master/LICENSE)
[![GitHub release](https://img.shields.io/github/release/bububa/ljson.svg)](https://GitHub.com/bububa/ljson/releases/)

## Install

Install the package into your code with:

```bash
go get "github.com/bububa/ljson"
```

Import in your code:

```go
import (
	"github.com/bububa/ljson"
)
```

## Example

```go
package main

import (
	"context"
	"fmt"
	"os"

	"github.com/bububa/ljson"
)

func main() {
	// Define the expected schema as a struct
	type Nested struct {
		A int `json:"a"`
	}

	type MySchema struct {
		Field1    []map[string]string `json:"field1"`
		NestedObj Nested              `json:"nested"`
		Numbers   int                 `json:"numbers"`
		BoolVal   bool                `json:"bool_val"`
	}

	// JSON with objects stored as strings and type mismatches
	jsonStr := `{
		"field1": "[{\"sub1\": \"xxx\"}, {\"sub2\": \"yyy\"}]",
    "nested": "{\"a\": \"123\"}",
		"numbers": "456",
		"bool_val": "true"
	}`
	arrStr := `[{
		"field1": "[{\"sub1\": \"xxx\"}, {\"sub2\": \"yyy\"}]",
    "nested": "{\"a\": \"123\"}",
		"numbers": "456",
		"bool_val": "true"
  }, {
		"field1": "[{\"sub1\": \"xxx\"}, {\"sub2\": \"yyy\"}]",
    "nested": "{\"a\": \"123\"}",
		"numbers": "456",
		"bool_val": "true"
  }]`
	mapStr := `"{\"sub1\": \"xxx\", \"sub2\": 123}"`

	// Define a struct instance to receive the parsed data
	var result MySchema

	// Unmarshal using our loose parser
	if err := ljson.Unmarshal([]byte(jsonStr), &result); err != nil {
		return
	}

	// Print the processed result
	resultJSON, _ := json.MarshalIndent(result, "", "  ")
	fmt.Println(string(resultJSON))

	// Define a struct instance to receive the parsed data
	var arrResult []MySchema

	// Unmarshal using our loose parser
	if err := l.Unmarshal([]byte(arrStr), &arrResult); err != nil {
		return
	}

	// Print the processed result
	resultJSON, _ = json.MarshalIndent(arrResult, "", "  ")
	fmt.Println(string(resultJSON))

	// Define a struct instance to receive the parsed data
	mapResult := make(map[string]string)

	// Unmarshal using our loose parser
	if err := l.Unmarshal([]byte(mapStr), &mapResult); err != nil {
		return
	}

	// Print the processed result
	resultJSON, _ = json.MarshalIndent(mapResult, "", "  ")
	fmt.Println(string(resultJSON))

	// Example with interface type
	interfaceData := `{
		"some_field": "{\"a\": \"456\"}"
	}`
	var myInterface interface{}
	if err := Unmarshal([]byte(interfaceData), &myInterface); err != nil {
		fmt.Println("Error unmarshalling interface:", err)
	} else {
		fmt.Printf("Unmarshalled interface: %+v\n", myInterface)
	}
```
