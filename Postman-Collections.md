{
  "info": {
    "name": "Digital Insurance Platform - API Tests",
    "description": "Postman collection for functional, negative and business-rule API testing of the Digital Insurance Platform.",
    "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
  },
  "variable": [
    {
      "key": "baseUrl",
      "value": "https://qa-api.example.com"
    },
    {
      "key": "customerId",
      "value": "10025"
    },
    {
      "key": "quoteId",
      "value": "Q-2026-000145"
    },
    {
      "key": "applicationId",
      "value": "APP-2026-001245"
    },
    {
      "key": "policyId",
      "value": "POL-2026-000981"
    }
  ],
  "item": [
    {
      "name": "Quote API",
      "item": [
        {
          "name": "TC-001 Create Quote - Valid Request",
          "request": {
            "method": "POST",
            "header": [
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "body": {
              "mode": "raw",
              "raw": "{\n  \"customerId\": {{customerId}},\n  \"vehicle\": {\n    \"plate\": \"35ABC123\",\n    \"brand\": \"Toyota\",\n    \"model\": \"Corolla\",\n    \"modelYear\": 2024,\n    \"vehicleType\": \"PASSENGER\"\n  },\n  \"product\": \"CASCO\"\n}"
            },
            "url": {
              "raw": "{{baseUrl}}/api/v1/quotes",
              "host": [
                "{{baseUrl}}"
              ],
              "path": [
                "api",
                "v1",
                "quotes"
              ]
            }
          },
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test(\"Status code is 201\", function () {",
                  "    pm.response.to.have.status(201);",
                  "});",
                  "",
                  "pm.test(\"Quote ID is returned\", function () {",
                  "    const json = pm.response.json();",
                  "    pm.expect(json.quoteId).to.exist;",
                  "});",
                  "",
                  "pm.test(\"Premium is greater than zero\", function () {",
                  "    const json = pm.response.json();",
                  "    pm.expect(json.premium).to.be.above(0);",
                  "});"
                ]
              }
            }
          ]
        },
        {
          "name": "TC-002 Create Quote - Missing Customer ID",
          "request": {
            "method": "POST",
            "header": [
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "body": {
              "mode": "raw",
              "raw": "{\n  \"vehicle\": {\n    \"plate\": \"35ABC123\",\n    \"brand\": \"Toyota\",\n    \"model\": \"Corolla\",\n    \"modelYear\": 2024\n  },\n  \"product\": \"CASCO\"\n}"
            },
            "url": {
              "raw": "{{baseUrl}}/api/v1/quotes",
              "host": [
                "{{baseUrl}}"
              ],
              "path": [
                "api",
                "v1",
                "quotes"
              ]
            }
          },
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test(\"Status code is 400\", function () {",
                  "    pm.response.to.have.status(400);",
                  "});",
                  "",
                  "pm.test(\"Correct error code is returned\", function () {",
                  "    const json = pm.response.json();",
                  "    pm.expect(json.errorCode).to.eql(\"CUSTOMER_ID_REQUIRED\");",
                  "});"
                ]
              }
            }
          ]
        },
        {
          "name": "TC-004 Create Quote - Vehicle Not Found",
          "request": {
            "method": "POST",
            "header": [
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "body": {
              "mode": "raw",
              "raw": "{\n  \"customerId\": {{customerId}},\n  \"vehicle\": {\n    \"plate\": \"99INVALID\",\n    \"brand\": \"Unknown\",\n    \"model\": \"Unknown\",\n    \"modelYear\": 2024\n  },\n  \"product\": \"CASCO\"\n}"
            },
            "url": {
              "raw": "{{baseUrl}}/api/v1/quotes",
              "host": [
                "{{baseUrl}}"
              ],
              "path": [
                "api",
                "v1",
                "quotes"
              ]
            }
          },
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test(\"Status code is 404\", function () {",
                  "    pm.response.to.have.status(404);",
                  "});",
                  "",
                  "pm.test(\"Vehicle not found error is returned\", function () {",
                  "    const json = pm.response.json();",
                  "    pm.expect(json.errorCode).to.eql(\"VEHICLE_NOT_FOUND\");",
                  "});"
                ]
              }
            }
          ]
        },
        {
          "name": "TC-008 Get Quote",
          "request": {
            "method": "GET",
            "url": {
              "raw": "{{baseUrl}}/api/v1/quotes/{{quoteId}}",
              "host": [
                "{{baseUrl}}"
              ],
              "path": [
                "api",
                "v1",
                "quotes",
                "{{quoteId}}"
              ]
            }
          },
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test(\"Status code is 200\", function () {",
                  "    pm.response.to.have.status(200);",
                  "});"
                ]
              }
            }
          ]
        }
      ]
    },
    {
      "name": "Application API",
      "item": [
        {
          "name": "TC-010 Create Application",
          "request": {
            "method": "POST",
            "header": [
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "body": {
              "mode": "raw",
              "raw": "{\n  \"quoteId\": \"{{quoteId}}\"\n}"
            },
            "url": {
              "raw": "{{baseUrl}}/api/v1/applications",
              "host": [
                "{{baseUrl}}"
              ],
              "path": [
                "api",
                "v1",
                "applications"
              ]
            }
          }
        },
        {
          "name": "TC-011 Create Application - Expired Quote",
          "request": {
            "method": "POST",
            "header": [
              {
                "key": "Content-Type",
                "value": "application/json"
              }
            ],
            "body": {
              "mode": "raw",
              "raw": "{\n  \"quoteId\": \"Q-EXPIRED-001\"\n}"
            },
            "url": {
              "raw": "{{baseUrl}}/api/v1/applications",
              "host": [
                "{{baseUrl}}"
              ],
              "path": [
                "api",
                "v1",
                "applications"
              ]
            }
          },
          "event": [
            {
              "listen": "test",
              "script": {
                "exec": [
                  "pm.test(\"Status code is 400\", function () {",
                  "
