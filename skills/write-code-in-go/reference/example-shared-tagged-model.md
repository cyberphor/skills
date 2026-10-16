# Shared Tagged Model

Use these generic examples for the preferred style. The files below form one small module named `example.com/app`. Adapt the endpoint, models, and behavior to the project. They illustrate an application entrypoint and HTTP integration, rather than command-line flag handling.


Keep struct definitions in `models`. Give exported types a doc comment and tag every field.

**models/record.go**

```go
package models

// Record describes a named resource.
type Record struct {
	ID   int    `mapstructure:"id" json:"id" yaml:"id"`
	Name string `mapstructure:"name" json:"name" yaml:"name"`
}
```

