# Application Orchestration

Use these generic examples for the preferred style. The files below form one small module named `example.com/app`. Adapt the endpoint, models, and behavior to the project. They illustrate an application entrypoint and HTTP integration, rather than command-line flag handling.


Keep configuration validation and orchestration in the application package. Declare local variables once, alphabetically, and return each error immediately without wrapping it.

**cmd/cmd.go**

```go
package cmd

import (
	"context"
	"encoding/json"
	"errors"
	"os"

	"example.com/app/models"
	"example.com/app/records"
)

// Execute fetches records from the configured endpoint and writes them as JSON.
func Execute(requestContext context.Context) error {
	var (
		endpoint     string
		err          error
		foundRecords []models.Record
	)

	endpoint = os.Getenv("RECORDS_ENDPOINT")
	if endpoint == "" {
		return errors.New("records endpoint is not configured")
	}

	foundRecords, err = records.Fetch(requestContext, endpoint)
	if err != nil {
		return err
	}

	err = json.NewEncoder(os.Stdout).Encode(foundRecords)
	if err != nil {
		return err
	}

	return nil
}
```

