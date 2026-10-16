# Standard HTTP and Focused Helpers

Use these generic examples for the preferred style. The files below form one small module named `example.com/app`. Adapt the endpoint, models, and behavior to the project. They illustrate an application entrypoint and HTTP integration, rather than command-line flag handling.


Use `net/http` and `net/url` directly. Separate URL construction, response validation, decoding, and cleanup into focused helpers. Check every returned error and close the response body on both successful and failed reads. Preserve a single existing error unchanged. Use `errors.Join` only when both the operation and cleanup fail.

**records/records.go**

```go
package records

import (
	"context"
	"encoding/json"
	"errors"
	"fmt"
	"io"
	"net/http"
	"net/url"
	"time"

	"example.com/app/models"
)

// Fetch retrieves the active records from an HTTP endpoint.
func Fetch(requestContext context.Context, endpoint string) ([]models.Record, error) {
	var (
		client       *http.Client
		err          error
		foundRecords []models.Record
		request      *http.Request
		requestURL   string
		response     *http.Response
	)

	requestURL, err = buildURL(endpoint)
	if err != nil {
		return nil, err
	}

	request, err = http.NewRequestWithContext(requestContext, http.MethodGet, requestURL, nil)
	if err != nil {
		return nil, err
	}

	client = &http.Client{Timeout: 10 * time.Second}
	response, err = client.Do(request)
	if err != nil {
		return nil, err
	}

	err = checkStatus(response)
	if err != nil {
		return nil, closeResponse(response, err)
	}

	foundRecords, err = decodeRecords(response.Body)
	if err != nil {
		return nil, closeResponse(response, err)
	}

	err = closeResponse(response, nil)
	if err != nil {
		return nil, err
	}

	return foundRecords, nil
}

func buildURL(endpoint string) (string, error) {
	var (
		err      error
		query    url.Values
		resource *url.URL
	)

	resource, err = url.Parse(endpoint)
	if err != nil {
		return "", err
	}

	query = resource.Query()
	query.Set("active", "true")
	resource.RawQuery = query.Encode()

	return resource.String(), nil
}

func checkStatus(response *http.Response) error {
	if response.StatusCode != http.StatusOK {
		return fmt.Errorf("unexpected response status: %d", response.StatusCode)
	}

	return nil
}

func decodeRecords(reader io.Reader) ([]models.Record, error) {
	var (
		err          error
		foundRecords []models.Record
	)

	err = json.NewDecoder(reader).Decode(&foundRecords)
	if err != nil {
		return nil, err
	}

	return foundRecords, nil
}

func closeResponse(response *http.Response, cause error) error {
	var (
		err error
	)

	err = response.Body.Close()
	if err != nil {
		if cause != nil {
			// Preserve both failures so callers can inspect each original error.
			return errors.Join(cause, err)
		}

		return err
	}

	return cause
}
```

These examples use Go 1.20 or later for `errors.Join`. Reuse a project-managed HTTP client when its lifecycle requires that. Command-line programs should use the Cobra group and leaf structure described in the main skill.

