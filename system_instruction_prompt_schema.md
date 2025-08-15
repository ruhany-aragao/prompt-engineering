{
  "name": "final_response",
  "strict": true,
  "schema": {
    "type": "object",
    "properties": {
      "result": {
        "type": "object",
        "description": "The output result object. Must be either type 'table' or 'text'.",
        "properties": {
          "type": {
            "type": "string",
            "description": "The kind of result this is.",
            "enum": [
              "table",
              "text"
            ]
          },
          "columns": {
            "type": ["array", "null"],
            "description": "Array of column definitions for table results. Each entry includes the column name and a concise explanation of its calculation or data source.",
            "items": {
              "type": "object",
              "properties": {
                "column_name": {
                  "type": "string",
                  "description": "The column name as presented in the table header.",
                  "minLength": 1
                },
                "explanation": {
                  "type": "string",
                  "description": "A concise plain-text explanation of the calculation and/or data sources.",
                  "minLength": 1
                },
                "formula": {
                  "type": ["string", "null"],
                  "description": "Markdown LaTeX formula describing the calculation if applicable (e.g., $$\\text{Margin} = \\frac{Revenue - Cost}{Revenue}$$). Always use display equation format with double dollar signs ($$). Use null when not applicable.",
                  "minLength": 1
                },
                "format": {
                  "type": "string",
                  "description": "Frontend formatting hint.",
                  "enum": ["float", "integer", "currency", "percentage", "date", "datetime", "string"]
                }
              },
              "required": [
                "column_name",
                "explanation",
                "formula",
                "format"
              ],
              "additionalProperties": false
            }
          },
          "top_5_rows": {
            "type": ["array", "null"],
            "description": "Array of top 5 rows of the table result. Each row is an array of values in the same order as the 'columns' array (matching each column object's position).",
            "items": {
              "type": "array",
              "items": {
                "type": ["string", "number", "boolean", "null"]
              }
            },
            "maxItems": 5
          },
          "value": {
            "type": ["string", "null"],
            "description": "The text value, only applied for results of type 'text'.",
            "minLength": 1
          },
          "file_path": {
            "type": "string",
            "description": "Filesystem path where the result was saved. Must be '/mnt/data/output.csv' for table or '/mnt/data/output.txt' for text. No other files should be created.",
            "enum": ["/mnt/data/output.csv", "/mnt/data/output.txt"]
          }
        },
        "required": [
          "type",
          "columns",
          "top_5_rows",
          "value",
          "file_path"
        ],
        "additionalProperties": false
      },
      "code": {
        "type": "string",
        "description": "Concise, self-contained Python code that reproduces the final result and writes it only to result.file_path (no other files). No network calls. Do not include a module guard (no if **name** == '**main**': or if name == 'main')."
      },
      "reasoning": {
        "type": "string",
        "description": "Explanation of the process. MUST include an explicit Plan outlining each step and a brief summary."
      },
      "result_was_saved_to_file": {
        "type": "boolean",
        "description": "True only if the agent executed the code and the file exists at result.file_path. Always present."
      }
    },
    "required": [
      "result",
      "code",
      "reasoning",
      "result_was_saved_to_file"
    ],
    "additionalProperties": false
  }
}