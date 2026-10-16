{
  "name": "AI Lead Qualification & Follow-Up System - Reply Assistant",
  "nodes": [
    {
      "parameters": {
        "pollTimes": {
          "item": [
            {
              "mode": "everyMinute"
            }
          ]
        },
        "documentId": {
          "__rl": true,
          "value": "1bEJAU8RsP7pjRvrXF7D6tuxRFv2h7Xn7HBKXf8qC7qM",
          "mode": "list",
          "cachedResultName": "EasyMarketPro Lead Form (Responses)",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1bEJAU8RsP7pjRvrXF7D6tuxRFv2h7Xn7HBKXf8qC7qM/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": 1889259625,
          "mode": "list",
          "cachedResultName": "Form Responses 1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1bEJAU8RsP7pjRvrXF7D6tuxRFv2h7Xn7HBKXf8qC7qM/edit#gid=1889259625"
        },
        "event": "rowAdded",
        "options": {}
      },
      "id": "cfeea740-e7ad-4a38-9b88-3634005cfe75",
      "name": "LeadCapture",
      "type": "n8n-nodes-base.googleSheetsTrigger",
      "typeVersion": 1,
      "position": [
        -2032,
        -288
      ],
      "credentials": {
        "googleSheetsTriggerOAuth2Api": {
          "id": "KZCiSdiQc9LDOyW2",
          "name": "Google Sheets Trigger account"
        }
      }
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "7ab005e6-ec9e-4fa5-856a-709a7128cec6",
              "name": "name",
              "value": "={{ $json['  Full Name  '] }}",
              "type": "string"
            },
            {
              "id": "62997032-5e32-4fdb-8b93-ddc918ac04e9",
              "name": "email",
              "value": "={{ $json['Business Email '] }}",
              "type": "string"
            },
            {
              "id": "a5d3d1fb-8bdf-4b64-9e26-477e68e49f54",
              "name": "company_name",
              "value": "={{ $json['  Company Name  '] }}",
              "type": "string"
            },
            {
              "id": "2e75b84f-bc2d-493b-a39c-b330da9ec419",
              "name": "website",
              "value": "={{ $json['  Website  '] }}",
              "type": "string"
            },
            {
              "id": "13b59495-f5c3-48d4-94f1-e4ea6c5de377",
              "name": "role",
              "value": "={{ $json['  Your Role  '].includes('Owner') ? 'decision-maker' \n   : $json['  Your Role  '].includes('Manager') ? 'influencer' \n   : 'other' }}",
              "type": "string"
            },
            {
              "id": "4b48d09b-d595-472b-9207-111e458a7fec",
              "name": "business_type",
              "value": "={{ $json['  What does your business do?  '] }}",
              "type": "string"
            },
            {
              "id": "d3ed3322-cad5-4e3e-bbe2-0ac2b4571271",
              "name": "raw_message",
              "value": "={{ $json['   What problem are you looking to solve?  '] }}",
              "type": "string"
            },
            {
              "id": "925a26d9-5506-4a0b-8bb9-1a54e53f009c",
              "name": "Timestamp",
              "value": "={{ $json.Timestamp }}",
              "type": "string"
            },
            {
              "id": "772ccceb-985b-4a42-a8c2-b613187f8c48",
              "name": "role_label",
              "value": "={{ $json['  Your Role  '].includes('Owner') ? 'Owner/Executive' \n   : $json['  Your Role  '].includes('Manager') ? 'Manager/Director' \n   : $json['  Your Role  '].includes('Team Member') ? 'Team Member' \n   : 'Other' }}",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "id": "6118971d-d116-40c6-885d-b9929c29d735",
      "name": "LeadExtraction",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.5,
      "position": [
        -1888,
        -288
      ]
    },
    {
      "parameters": {
        "modelId": {
          "__rl": true,
          "value": "chat-latest",
          "mode": "list",
          "cachedResultName": "CHAT-LATEST"
        },
        "responses": {
          "values": [
            {
              "role": "system",
              "content": "You are a lead qualification system for EasyMarketPro Limited, an AI automation agency.\n\nYour task is to determine whether an incoming lead is qualified based on whether their business has a customer support or customer service problem that EasyMarketPro could potentially solve with AI automation.\n\nAnalyze the lead's business type, role, and stated problem.\n\nA lead should be considered qualified when their stated problem is clearly or reasonably related to customer support/service and could potentially be addressed through AI automation.\n\nReturn:\n- qualified: true or false\n- qualification_reason: a brief explanation\n- pain_point_summary: a short, natural-language summary of the lead's problem\n- problem_clarity: \"clear\", \"related\", or \"not_relevant\"\n\nDo not invent information that the lead did not provide."
            },
            {
              "content": "=Business Type: {{ $json.business_type }}\nRole: {{ $json.role }}\nProblem: {{ $json[\"raw_message\"] }}"
            }
          ]
        },
        "builtInTools": {},
        "options": {
          "textFormat": {
            "textOptions": {
              "type": "json_schema"
            }
          }
        }
      },
      "id": "9d020d7f-7715-4aff-bc62-a20d88527c05",
      "name": "LeadQualification",
      "type": "@n8n/n8n-nodes-langchain.openAi",
      "typeVersion": 2.3,
      "position": [
        -1760,
        -288
      ],
      "credentials": {
        "openAiApi": {
          "id": "Iuj3PecT9isReL1E",
          "name": "OpenAI account"
        }
      }
    },
    {
      "parameters": {
        "mode": "runOnceForEachItem",
        "jsCode": "const raw = $json.output[0].content[0].text.message;\n\nlet parsed;\ntry {\n  parsed = JSON.parse(raw);\n} catch (e) {\n  parsed = {\n    qualified: false,\n    qualification_reason: \"PARSE_ERROR: could not parse AI response\",\n    pain_point_summary: \"\",\n    problem_clarity: \"not_relevant\"\n  };\n}\n\nreturn {\n  name: $('LeadExtraction').item.json.name,\n  email: $('LeadExtraction').item.json.email,\n  company_name: $('LeadExtraction').item.json.company_name,\n  website: $('LeadExtraction').item.json.website,\n  role: $('LeadExtraction').item.json.role,\n  business_type: $('LeadExtraction').item.json.business_type,\n  raw_message: $('LeadExtraction').item.json.raw_message,\n  Timestamp: $('LeadExtraction').item.json.Timestamp,\n  qualified: parsed.qualified,\n  qualification_reason: parsed.qualification_reason,\n  pain_point_summary: parsed.pain_point_summary,\n  problem_clarity: parsed.problem_clarity\n};"
      },
      "id": "4709d148-8e86-4b5f-aaab-9362a889d5e8",
      "name": "QualificationParse",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1488,
        -288
      ]
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "a92a9883-666e-42c1-a49a-b4eb871daf03",
              "leftValue": "={{ $json.qualified }}",
              "rightValue": true,
              "operator": {
                "type": "boolean",
                "operation": "equals"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "id": "a3c0d5ae-364a-4898-92cd-c041ee73ec58",
      "name": "If",
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        -1328,
        -288
      ]
    },
    {
      "parameters": {
        "mode": "runOnceForEachItem",
        "jsCode": "const role = $json.role;\nconst problem_clarity = $json.problem_clarity;\n\nlet roleScore = 0;\nif (role === 'decision-maker') roleScore = 50;\nelse if (role === 'influencer') roleScore = 35;\nelse roleScore = 10;\n\nlet problemScore = 0;\nif (problem_clarity === 'clear') problemScore = 50;\nelse if (problem_clarity === 'related') problemScore = 25;\nelse problemScore = 0;\n\nconst score = roleScore + problemScore;\n\nlet priority = 'LOW PRIORITY';\nif (score >= 80) priority = 'HOT';\nelse if (score >= 50) priority = 'NURTURE';\n\nreturn {\n  ...$json,\n  lead_score: score,\n  lead_priority: priority\n};"
      },
      "id": "dedd765c-7a2b-4d84-9c30-1f70da438867",
      "name": "LeadScoring",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1152,
        -416
      ]
    },
    {
      "parameters": {
        "authentication": "airtableOAuth2Api",
        "operation": "create",
        "base": {
          "__rl": true,
          "value": "appkFksjr82WRknGp",
          "mode": "list",
          "cachedResultName": "Untitled Base",
          "cachedResultUrl": "https://airtable.com/appkFksjr82WRknGp"
        },
        "table": {
          "__rl": true,
          "value": "tblC1UIxsAoJbuPQF",
          "mode": "list",
          "cachedResultName": "Leads",
          "cachedResultUrl": "https://airtable.com/appkFksjr82WRknGp/tblC1UIxsAoJbuPQF"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "Name": "={{ $json.name }}",
            "Company": "={{ $json.company_name }}",
            "Email": "={{ $json.email }}",
            "Website": "={{ $json.website }}",
            "Role": "={{ $('LeadExtraction').item.json.role_label }}",
            "Problem (Raw)": "={{ $json.raw_message }}",
            "Business Type": "={{ $json.business_type }}",
            "Pain Point Summary": "={{ $json.pain_point_summary }}",
            "Qualification Reason": "={{ $json.qualification_reason }}",
            "Problem Clarity": "={{ $json.problem_clarity === 'clear' ? 'Clear' : $json.problem_clarity === 'related' ? 'Related' : 'Not Relevant' }}",
            "Lead Score": "={{ $json.lead_score }}",
            "Lead Priority": "={{ $json.lead_priority }}",
            "Qualification": "Qualified",
            "Status": "New",
            "Lead Source": "Google Form",
            "Created At": "={{ $('LeadExtraction').item.json.Timestamp }}",
            "Follow-up Count": 0
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "Name",
              "displayName": "Name",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Email",
              "displayName": "Email",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Company",
              "displayName": "Company",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Website",
              "displayName": "Website",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Role",
              "displayName": "Role",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "options",
              "options": [
                {
                  "name": "Owner/Executive",
                  "value": "Owner/Executive"
                },
                {
                  "name": "Manager/Director",
                  "value": "Manager/Director"
                },
                {
                  "name": "Team Member",
                  "value": "Team Member"
                },
                {
                  "name": "Other",
                  "value": "Other"
                }
              ],
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Business Type",
              "displayName": "Business Type",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Problem (Raw)",
              "displayName": "Problem (Raw)",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Qualification",
              "displayName": "Qualification",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "options",
              "options": [
                {
                  "name": "Qualified",
                  "value": "Qualified"
                },
                {
                  "name": "Disqualified",
                  "value": "Disqualified"
                }
              ],
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Qualification Reason",
              "displayName": "Qualification Reason",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Pain Point Summary",
              "displayName": "Pain Point Summary",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Problem Clarity",
              "displayName": "Problem Clarity",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "options",
              "options": [
                {
                  "name": "Clear",
                  "value": "Clear"
                },
                {
                  "name": "Related",
                  "value": "Related"
                },
                {
                  "name": "Not Relevant",
                  "value": "Not Relevant"
                }
              ],
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Lead Score",
              "displayName": "Lead Score",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "number",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Lead Priority",
              "displayName": "Lead Priority",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "options",
              "options": [
                {
                  "name": "HOT",
                  "value": "HOT"
                },
                {
                  "name": "NURTURE",
                  "value": "NURTURE"
                },
                {
                  "name": "LOW PRIORITY",
                  "value": "LOW PRIORITY"
                }
              ],
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Status",
              "displayName": "Status",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "options",
              "options": [
                {
                  "name": "New",
                  "value": "New"
                },
                {
                  "name": "Nurturing",
                  "value": "Nurturing"
                },
                {
                  "name": "Follow-up Sent",
                  "value": "Follow-up Sent"
                },
                {
                  "name": "Re-engagement Sent",
                  "value": "Re-engagement Sent"
                },
                {
                  "name": "Booked",
                  "value": "Booked"
                },
                {
                  "name": "Disqualified",
                  "value": "Disqualified"
                },
                {
                  "name": "Awaiting Booking",
                  "value": "Awaiting Booking"
                }
              ],
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Lead Source",
              "displayName": "Lead Source",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "options",
              "options": [
                {
                  "name": "Google Form",
                  "value": "Google Form"
                }
              ],
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Created At",
              "displayName": "Created At",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "dateTime",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Follow-up Count",
              "displayName": "Follow-up Count",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "number",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "Last Contacted",
              "displayName": "Last Contacted",
              "required": false,
              "defaultMatch": false,
              "canBeUsedToMatch": true,
              "display": true,
              "type": "dateTime",
              "readOnly": false,
              "removed": false
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "id": "ac1184f2-fa76-4e7b-bb9d-41eda31daf8d",
      "name": "QualifiedLeads",
      "type": "n8n-nodes-base.airtable",
      "typeVersion": 2.2,
      "position": [
        -928,
        -416
      ],
      "credentials": {
        "airtableOAuth2Api": {
          "id": "XJFBTNiaiIQT42jz",
          "name": "Airtable account 3"
        }
      }
    },
    {
      "parameters": {
        "rules": {
          "values": [
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.fields['Lead Priority'] === 'HOT' }}",
                    "rightValue": "HOT",
                    "operator": {
                      "type": "boolean",
                      "operation": "true",
                      "singleValue": true
                    },
                    "id": "7f2d1b99-afd5-4c65-be49-518c7e1a37d1"
                  }
                ],
                "combinator": "and"
              }
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "031a2e04-34d5-45d3-984c-ebb1509a4613",
                    "leftValue": "={{ $json.fields['Lead Priority'] === 'NURTURE' || $json.fields['Lead Priority'] === 'LOW PRIORITY' }}",
                    "rightValue": "",
                    "operator": {
                      "type": "boolean",
                      "operation": "true",
                      "singleValue": true
                    }
                  }
                ],
                "combinator": "and"
              }
            }
          ]
        },
        "options": {}
      },
      "id": "83f7cedb-1859-4a0c-9c0b-cf58178b52da",
      "name": "PriorityRouter",
      "type": "n8n-nodes-base.switch",
      "typeVersion": 3.4,
      "position": [
        -704,
        -416
      ]
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "3bed99ee-52df-4fac-a453-ff7b79fac9f2",
              "name": "email_body",
              "value": "=Hi {{ $json.fields.Name }}, thanks for sharing that. Based on what you described about {{ $json.fields['Pain Point Summary'] }}, this is something we can help you automate.  I'd be happy to show you what that could look like for {{ $json.fields.Company }}. You can book a quick call here: https://calendly.com/jerrybyang2/30min",
              "type": "string"
            },
            {
              "id": "8fe3310c-7dec-4b3c-a530-502a1260152f",
              "name": "email_subject",
              "value": "={{ 'Quick question about ' + $json.fields.Company }}",
              "type": "string"
            },
            {
              "id": "f43e3c18-3db0-4274-9606-320e3355b86a",
              "name": "email",
              "value": "={{ $json.fields.Email }}",
              "type": "string"
            },
            {
              "id": "b724ad3a-96fa-4a9c-a747-d4f2470e4aad",
              "name": "record_id",
              "value": "={{ $json.id }}",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "id": "114e73aa-8932-4c67-be2a-05cd1362ea85",
      "name": "ComposeHotMessage",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.5,
      "position": [
        -496,
        -528
      ]
    },
    {
      "parameters": {
        "sendTo": "={{ $json.email }}",
        "subject": "={{ $json.email_subject }}",
        "emailType": "text",
        "message": "={{ $json.email_body }}",
        "options": {
          "appendAttribution": false
        }
      },
      "id": "c446e9a0-414c-46c7-b0a4-7de1c0001d7f",
      "name": "MessageHotLead",
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.2,
      "position": [
        -272,
        -544
      ],
      "webhookId": "3ef5240e-e2cd-4878-9fa7-d7a7b1c27338",
      "credentials": {
        "gmailOAuth2": {
          "id": "VKZ04lF3HwpvlQVn",
          "name": "Gmail account 4"
        }
      }
    },
    {
      "parameters": {
        "authentication": "airtableOAuth2Api",
        "operation": "update",
        "base": {
          "__rl": true,
          "value": "appkFksjr82WRknGp",
          "mode": "list",
          "cachedResultName": "Untitled Base",
          "cachedResultUrl": "https://airtable.com/appkFksjr82WRknGp"
        },
        "table": {
          "__rl": true,
          "value": "tblC1UIxsAoJbuPQF",
          "mode": "list",
          "cachedResultName": "Leads",
          "cachedResultUrl": "https://airtable.com/appkFksjr82WRknGp/tblC1UIxsAoJbuPQF"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "id": "={{ $('ComposeHotMessage').item.json.record_id }}",
            "Status": "Awaiting Booking",
            "Lead Score": 0,
            "Follow-up Count": 0
          },
          "matchingColumns": [
            "id"
          ],
          "schema": []
        },
        "options": {}
      },
      "id": "0d371e43-0b46-4cf9-ba7f-7c07e7a95424",
      "name": "UpdateHotLeads",
      "type": "n8n-nodes-base.airtable",
      "typeVersion": 2.2,
      "position": [
        -64,
        -544
      ],
      "credentials": {
        "airtableOAuth2Api": {
          "id": "XJFBTNiaiIQT42jz",
          "name": "Airtable account 3"
        }
      }
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "990c5d85-8761-4595-8c68-d9dcaf550b41",
              "name": "email_body",
              "value": "=Hi {{ $json.fields.Name }}, thanks for sharing that. Based on what you described, there may be a few opportunities to use automation to improve {{ $json.fields['Pain Point Summary'] }}.  For businesses dealing with similar challenges, we typically look at where repetitive work is consuming the most time and identify what can realistically be automated. I'll be happy to share some ideas when you're ready.",
              "type": "string"
            },
            {
              "id": "b33bb89c-0353-45f0-b364-ef3644cf94c5",
              "name": "email_subject",
              "value": "={{ 'A few ideas for ' + $json.fields.Company }}",
              "type": "string"
            },
            {
              "id": "5f35472f-1117-4132-95b4-01871e5692eb",
              "name": "email",
              "value": "={{ $json.fields.Email }}",
              "type": "string"
            },
            {
              "id": "1e763f76-f868-432f-827b-9c160abefcce",
              "name": "record_id",
              "value": "={{ $json.id }}",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "id": "20e14896-67e1-4ac8-bc72-63555026bc80",
      "name": "ComposeNurtureMessage",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.5,
      "position": [
        -496,
        -304
      ]
    },
    {
      "parameters": {
        "sendTo": "={{ $json.email }}",
        "subject": "={{ $json.email_subject }}",
        "emailType": "text",
        "message": "={{ $json.email_body }}",
        "options": {
          "appendAttribution": false
        }
      },
      "id": "80fd1cf7-7591-48fc-b6c0-200feaf7b6e8",
      "name": "MessageNurtureLead",
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.2,
      "position": [
        -256,
        -304
      ],
      "webhookId": "96e76057-6c5d-466e-b4f0-bf533f23747f",
      "credentials": {
        "gmailOAuth2": {
          "id": "VKZ04lF3HwpvlQVn",
          "name": "Gmail account 4"
        }
      }
    },
    {
      "parameters": {
        "authentication": "airtableOAuth2Api",
        "operation": "update",
        "base": {
          "__rl": true,
          "value": "appkFksjr82WRknGp",
          "mode": "list",
          "cachedResultName": "Untitled Base",
          "cachedResultUrl": "https://airtable.com/appkFksjr82WRknGp"
        },
        "table": {
          "__rl": true,
          "value": "tblC1UIxsAoJbuPQF",
          "mode": "list",
          "cachedResultName": "Leads",
          "cachedResultUrl": "https://airtable.com/appkFksjr82WRknGp/tblC1UIxsAoJbuPQF"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "id": "={{ $('ComposeNurtureMessage').item.json.record_id }}",
            "Lead Score": 0,
            "Follow-up Count": 0,
            "Status": "Nurturing"
          },
          "matchingColumns": [
            "id"
          ],
          "schema": []
        },
        "options": {}
      },
      "id": "8103305b-001d-4f12-aded-5ede6c0137e1",
      "name": "UpdateNurtureLead",
      "type": "n8n-nodes-base.airtable",
      "typeVersion": 2.2,
      "position": [
        -32,
        -304
      ],
      "credentials": {
        "airtableOAuth2Api": {
          "id": "XJFBTNiaiIQT42jz",
          "name": "Airtable account 3"
        }
      }
    },
    {
      "parameters": {
        "authentication": "airtableOAuth2Api",
        "operation": "create",
        "base": {
          "__rl": true,
          "value": "=appkFksjr82WRknGp",
          "mode": "id"
        },
        "table": {
          "__rl": true,
          "value": "tblC1UIxsAoJbuPQF",
          "mode": "list",
          "cachedResultName": "Leads",
          "cachedResultUrl": "https://airtable.com/appkFksjr82WRknGp/tblC1UIxsAoJbuPQF"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "Name": "={{ $('LeadExtraction').item.json.name }}",
            "Email": "={{ $('LeadExtraction').item.json.email }}",
            "Company": "={{ $('LeadExtraction').item.json.company_name }}",
            "Website": "={{ $('LeadExtraction').item.json.website }}",
            "Role": "={{ $('LeadExtraction').item.json.role_label }}",
            "Business Type": "={{ $('LeadExtraction').item.json.business_type }}",
            "Problem (Raw)": "={{ $('LeadExtraction').item.json.raw_message }}",
            "Pain Point Summary": "={{ $('QualificationParse').item.json.pain_point_summary }}",
            "Qualification": "={{ $('QualificationParse').item.json.qualified ? 'Qualified' : 'Disqualified' }}",
            "Qualification Reason": "={{ $('QualificationParse').item.json.qualification_reason }}",
            "Problem Clarity": "={{ $('QualificationParse').item.json.problem_clarity === 'clear' ? 'Clear' : $('QualificationParse').item.json.problem_clarity === 'related' ? 'Related' : 'Not Relevant' }}",
            "Created At": "={{ $('QualificationParse').item.json.Timestamp }}",
            "Lead Source": "Google Form",
            "Status": "Disqualified"
          },
          "matchingColumns": [],
          "schema": []
        },
        "options": {}
      },
      "id": "b63380b2-31e1-40ce-b666-5fce81a3895d",
      "name": "DisqualifiedLeads",
      "type": "n8n-nodes-base.airtable",
      "typeVersion": 2.2,
      "position": [
        -1152,
        -224
      ],
      "credentials": {
        "airtableOAuth2Api": {
          "id": "XJFBTNiaiIQT42jz",
          "name": "Airtable account 3"
        }
      }
    },
    {
      "parameters": {
        "content": "LEAD QUALIFICATION AND SCORING WORKFLOW ",
        "height": 512,
        "width": 2272
      },
      "id": "cc73bf4e-b524-45f5-8193-1cb001198809",
      "name": "Sticky Note",
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        -2064,
        -576
      ]
    },
    {
      "parameters": {
        "content": "FOLLOW UP WORKFLOW\n",
        "height": 592,
        "width": 2272,
        "color": 4
      },
      "id": "5dbc62d0-3b16-4531-ab2f-72e2b21275d8",
      "name": "Sticky Note1",
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        -2064,
        -64
      ]
    },
    {
      "parameters": {
        "rule": {
          "interval": [
            {}
          ]
        }
      },
      "id": "51ed61af-5198-420d-ac43-cce1323a84b8",
      "name": "Check Google calender",
      "type": "n8n-nodes-base.scheduleTrigger",
      "typeVersion": 1.3,
      "position": [
        -2016,
        640
      ]
    },
    {
      "parameters": {
        "operation": "getAll",
        "calendar": {
          "__rl": true,
          "value": "jefferythom217@gmail.com",
          "mode": "list",
          "cachedResultName": "jefferythom217@gmail.com"
        },
        "returnAll": true,
        "timeMin": "={{ $now.minus({ hours: 24 }).toISO() }}",
        "options": {}
      },
      "id": "e8e46afc-f71d-4ed6-8117-ed03323bc688",
      "name": "GetBookings",
      "type": "n8n-nodes-base.googleCalendar",
      "typeVersion": 1.3,
      "position": [
        -1856,
        640
      ],
      "credentials": {
        "googleCalendarOAuth2Api": {
          "id": "f2ZP4XWn6l3iSJuS",
          "name": "Google Calendar account 3"
        }
      }
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "15377df6-6855-45bc-883d-fc95c8180157",
              "name": "invitee_email",
              "value": "={{ $json.attendees.find(a => !a.organizer).email }}",
              "type": "string"
            }
          ]
        },
        "includeOtherFields": "={{ true }}",
        "options": {}
      },
      "id": "52f36cb9-efda-4367-9108-80d3656033eb",
      "name": "ExtractInvitee",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.5,
      "position": [
        -1696,
        640
      ]
    },
    {
      "parameters": {
        "authentication": "airtableOAuth2Api",
        "operation": "search",
        "base": {
          "__rl": true,
          "value": "appkFksjr82WRknGp",
          "mode": "list",
          "cachedResultName": "Untitled Base",
          "cachedResultUrl": "https://airtable.com/appkFksjr82WRknGp"
        },
        "table": {
          "__rl": true,
          "value": "tblC1UIxsAoJbuPQF",
          "mode": "list",
          "cachedResultName": "Leads",
          "cachedResultUrl": "https://airtable.com/appkFksjr82WRknGp/tblC1UIxsAoJbuPQF"
        },
        "filterByFormula": "={{ '{Email} = \"' + $json.invitee_email + '\"' }}",
        "options": {}
      },
      "id": "b32ce072-3bea-4c91-ad8d-a0d6ca071e50",
      "name": "FindBookedLead",
      "type": "n8n-nodes-base.airtable",
      "typeVersion": 2.2,
      "position": [
        -1520,
        640
      ],
      "credentials": {
        "airtableOAuth2Api": {
          "id": "XJFBTNiaiIQT42jz",
          "name": "Airtable account 3"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "9e775665-b2aa-4c47-8708-f4a7ae5b6883",
              "leftValue": "={{ $json.fields.Status }}",
              "rightValue": "Booked",
              "operator": {
                "type": "string",
                "operation": "notEquals"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "id": "e05de291-000d-4369-8b07-e12c3ade2aa9",
      "name": "IsNotBookedYet",
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        -1376,
        640
      ]
    },
    {
      "parameters": {
        "authentication": "airtableOAuth2Api",
        "operation": "update",
        "base": {
          "__rl": true,
          "value": "appkFksjr82WRknGp",
          "mode": "list",
          "cachedResultName": "Untitled Base",
          "cachedResultUrl": "https://airtable.com/appkFksjr82WRknGp"
        },
        "table": {
          "__rl": true,
          "value": "tblC1UIxsAoJbuPQF",
          "mode": "list",
          "cachedResultName": "Leads",
          "cachedResultUrl": "https://airtable.com/appkFksjr82WRknGp/tblC1UIxsAoJbuPQF"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "id": "={{ $json.id }}",
            "Status": "Booked",
            "Lead Score": 0,
            "Follow-up Count": 0
          },
          "matchingColumns": [
            "id"
          ],
          "schema": []
        },
        "options": {}
      },
      "id": "ff1d145b-4ed5-4b08-af9b-7595ccce5736",
      "name": "UpdateStatusBooked",
      "type": "n8n-nodes-base.airtable",
      "typeVersion": 2.2,
      "position": [
        -1184,
        560
      ],
      "credentials": {
        "airtableOAuth2Api": {
          "id": "XJFBTNiaiIQT42jz",
          "name": "Airtable account 3"
        }
      }
    },
    {
      "parameters": {
        "rule": {
          "interval": [
            {}
          ]
        }
      },
      "id": "f5036946-edce-455f-9e79-e129b9016289",
      "name": "ReEngagementSchedule",
      "type": "n8n-nodes-base.scheduleTrigger",
      "typeVersion": 1.2,
      "position": [
        -2064,
        240
      ]
    },
    {
      "parameters": {
        "authentication": "airtableOAuth2Api",
        "operation": "search",
        "base": {
          "__rl": true,
          "value": "appkFksjr82WRknGp",
          "mode": "list",
          "cachedResultName": "Untitled Base"
        },
        "table": {
          "__rl": true,
          "value": "tblC1UIxsAoJbuPQF",
          "mode": "list",
          "cachedResultName": "Leads"
        },
        "filterByFormula": "=OR(AND({Status} = \"Nurturing\", {Qualification} = \"Qualified\"), AND({Status} = \"Awaiting Booking\", {Qualification} = \"Qualified\"))",
        "options": {}
      },
      "id": "1fddba0f-1823-4021-a52b-86025b83e778",
      "name": "QueryFollowUpLeads1",
      "type": "n8n-nodes-base.airtable",
      "typeVersion": 2.2,
      "position": [
        -1824,
        240
      ],
      "credentials": {
        "airtableOAuth2Api": {
          "id": "XJFBTNiaiIQT42jz",
          "name": "Airtable account 3"
        }
      }
    },
    {
      "parameters": {
        "mode": "runOnceForEachItem",
        "jsCode": "const status = $json.fields.Status;\nconst followUpCount = $json.fields['Follow-up Count'] || 0;\n\nlet stage;\n\nif (status === 'Awaiting Booking') {\n  stage = followUpCount === 0 ? 'hot_followup' : 'stop';\n} else if (status === 'Nurturing') {\n  if (followUpCount === 0) stage = 'nurture_followup';\n  else if (followUpCount === 1) stage = 'final_reengagement';\n  else stage = 'stop';\n} else {\n  stage = 'stop';\n}\n\nreturn {\n  ...$json,\n  stage\n};"
      },
      "id": "0920ee33-fc78-49af-a114-2649622bea7d",
      "name": "DetermineStage1",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1344,
        112
      ]
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "cond-1",
              "leftValue": "={{ $json.stage }}",
              "rightValue": "stop",
              "operator": {
                "type": "string",
                "operation": "notEquals"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "id": "bd7a7517-649c-4110-8412-55801924ff9f",
      "name": "ShouldSend",
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        -1104,
        112
      ]
    },
    {
      "parameters": {
        "rules": {
          "values": [
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "r1",
                    "leftValue": "={{ $json.stage }}",
                    "rightValue": "hot_followup",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    }
                  }
                ],
                "combinator": "and"
              }
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "r2",
                    "leftValue": "={{ $json.stage }}",
                    "rightValue": "nurture_followup",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    }
                  }
                ],
                "combinator": "and"
              }
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "r3",
                    "leftValue": "={{ $json.stage }}",
                    "rightValue": "final_reengagement",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    }
                  }
                ],
                "combinator": "and"
              }
            }
          ]
        },
        "options": {}
      },
      "id": "eff84a9c-07b9-439e-94c0-a9cbef40d810",
      "name": "RouteStage",
      "type": "n8n-nodes-base.switch",
      "typeVersion": 3.4,
      "position": [
        -864,
        112
      ]
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "h1",
              "name": "email_body",
              "value": "=Hi {{ $json.fields.Name }}, just following up on my last message. I'd be happy to walk you through how we could approach {{ $json.fields['Pain Point Summary'] }} for {{ $json.fields.Company }}.\n\nIf you'd like to explore it, you can book a quick call here: [YOUR_CALENDLY_LINK]",
              "type": "string"
            },
            {
              "id": "h2",
              "name": "email_subject",
              "value": "={{ 'Following up — ' + $json.fields.Company }}",
              "type": "string"
            },
            {
              "id": "h3",
              "name": "email",
              "value": "={{ $json.fields.Email }}",
              "type": "string"
            },
            {
              "id": "h4",
              "name": "record_id",
              "value": "={{ $json.id }}",
              "type": "string"
            },
            {
              "id": "h5",
              "name": "new_status",
              "value": "Follow-up Sent",
              "type": "string"
            }
          ]
        },
        "includeOtherFields": true,
        "options": {}
      },
      "id": "d950a379-09a9-43e4-aa1c-0de1644fe4cb",
      "name": "ComposeHotFollowup",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.5,
      "position": [
        -624,
        -48
      ]
    },
    {
      "parameters": {
        "sendTo": "={{ $json.email }}",
        "subject": "={{ $json.email_subject }}",
        "message": "={{ $json.email_body }}",
        "options": {
          "appendAttribution": false
        }
      },
      "id": "c59130f5-9ac4-441c-b736-ff5fa23eea29",
      "name": "GmailHotFollowup",
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.1,
      "position": [
        -384,
        -48
      ],
      "webhookId": "64555696-4040-434a-913b-e75f1bda83db",
      "credentials": {
        "gmailOAuth2": {
          "id": "BjRArWx5GuYUsZZg",
          "name": "Gmail account 5"
        }
      }
    },
    {
      "parameters": {
        "authentication": "airtableOAuth2Api",
        "operation": "update",
        "base": {
          "__rl": true,
          "value": "appkFksjr82WRknGp",
          "mode": "list",
          "cachedResultName": "Untitled Base"
        },
        "table": {
          "__rl": true,
          "value": "tblC1UIxsAoJbuPQF",
          "mode": "list",
          "cachedResultName": "Leads"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "id": "={{ $('ComposeHotFollowup').item.json.record_id }}",
            "Status": "={{ $('ComposeHotFollowup').item.json.new_status }}",
            "Follow-up Count": "={{ ($('ComposeHotFollowup').item.json.fields['Follow-up Count'] || 0) + 1 }}",
            "Last Contacted": "={{ $now }}",
            "Lead Score": 0
          },
          "matchingColumns": [
            "id"
          ],
          "schema": []
        },
        "options": {}
      },
      "id": "f9b2e37b-f3b0-4ecd-9912-671f42452cd8",
      "name": "UpdateHotTracking",
      "type": "n8n-nodes-base.airtable",
      "typeVersion": 2.2,
      "position": [
        -144,
        -48
      ],
      "credentials": {
        "airtableOAuth2Api": {
          "id": "XJFBTNiaiIQT42jz",
          "name": "Airtable account 3"
        }
      }
    },
    {
      "parameters": {
        "options": {}
      },
      "id": "3fc1f9fe-539c-4ae4-92df-d19f2bd0febc",
      "name": "LoopLeads1",
      "type": "n8n-nodes-base.splitInBatches",
      "typeVersion": 3,
      "position": [
        -1584,
        240
      ]
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "n1",
              "name": "email_body",
              "value": "=Hi {{ $json.fields.Name }}, just checking in. Have you had a chance to think about {{ $json.fields['Pain Point Summary'] }}?\n\nIf it's still something you're looking to improve, I'd be happy to discuss a few practical ways automation could help.",
              "type": "string"
            },
            {
              "id": "n2",
              "name": "email_subject",
              "value": "={{ 'Checking in — ' + $json.fields.Company }}",
              "type": "string"
            },
            {
              "id": "n3",
              "name": "email",
              "value": "={{ $json.fields.Email }}",
              "type": "string"
            },
            {
              "id": "n4",
              "name": "record_id",
              "value": "={{ $json.id }}",
              "type": "string"
            },
            {
              "id": "n5",
              "name": "new_status",
              "value": "Nurturing",
              "type": "string"
            }
          ]
        },
        "includeOtherFields": true,
        "options": {}
      },
      "id": "2f22e6a2-0b31-4416-bd7b-213b95bd8bbd",
      "name": "ComposeNurtureFollowup",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.5,
      "position": [
        -624,
        112
      ]
    },
    {
      "parameters": {
        "sendTo": "={{ $json.email }}",
        "subject": "={{ $json.email_subject }}",
        "message": "={{ $json.email_body }}",
        "options": {
          "appendAttribution": false
        }
      },
      "id": "632dfb8a-010a-450e-912c-e649cc30f797",
      "name": "GmailNurtureFollowup",
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.1,
      "position": [
        -384,
        112
      ],
      "webhookId": "81108237-a2e7-431c-bd6f-88e913b08575",
      "credentials": {
        "gmailOAuth2": {
          "id": "U4PCI0t9RptOHFrU",
          "name": "Gmail account"
        }
      }
    },
    {
      "parameters": {
        "authentication": "airtableOAuth2Api",
        "operation": "update",
        "base": {
          "__rl": true,
          "value": "appkFksjr82WRknGp",
          "mode": "list",
          "cachedResultName": "Untitled Base"
        },
        "table": {
          "__rl": true,
          "value": "tblC1UIxsAoJbuPQF",
          "mode": "list",
          "cachedResultName": "Leads"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "id": "={{ $('ComposeNurtureFollowup').item.json.record_id }}",
            "Status": "={{ $('ComposeNurtureFollowup').item.json.new_status }}",
            "Follow-up Count": "={{ ($('ComposeNurtureFollowup').item.json.fields['Follow-up Count'] || 0) + 1 }}",
            "Last Contacted": "={{ $now }}",
            "Lead Score": 0
          },
          "matchingColumns": [
            "id"
          ],
          "schema": []
        },
        "options": {}
      },
      "id": "9bb83251-0873-48e9-a8c9-563b1d6c33b7",
      "name": "UpdateNurtureTracking",
      "type": "n8n-nodes-base.airtable",
      "typeVersion": 2.2,
      "position": [
        -144,
        112
      ],
      "credentials": {
        "airtableOAuth2Api": {
          "id": "XJFBTNiaiIQT42jz",
          "name": "Airtable account 3"
        }
      }
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "f1",
              "name": "email_body",
              "value": "=Hi {{ $json.fields.Name }}, I'll leave this here for now. If improving {{ $json.fields['Pain Point Summary'] }} becomes a priority, feel free to reach out and we can explore what could be automated for {{ $json.fields.Company }}.",
              "type": "string"
            },
            {
              "id": "f2",
              "name": "email_subject",
              "value": "={{ 'One last thought for ' + $json.fields.Company }}",
              "type": "string"
            },
            {
              "id": "f3",
              "name": "email",
              "value": "={{ $json.fields.Email }}",
              "type": "string"
            },
            {
              "id": "f4",
              "name": "record_id",
              "value": "={{ $json.id }}",
              "type": "string"
            },
            {
              "id": "f5",
              "name": "new_status",
              "value": "Re-engagement Sent",
              "type": "string"
            }
          ]
        },
        "includeOtherFields": true,
        "options": {}
      },
      "id": "2c284e0a-919a-4b3a-9b08-5a3dfe81b351",
      "name": "ComposeFinalReengagement",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.5,
      "position": [
        -624,
        272
      ]
    },
    {
      "parameters": {
        "sendTo": "={{ $json.email }}",
        "subject": "={{ $json.email_subject }}",
        "message": "={{ $json.email_body }}",
        "options": {
          "appendAttribution": false
        }
      },
      "id": "2cb64a26-fd67-42ef-afa0-f780c501314b",
      "name": "GmailFinalReengagement",
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.1,
      "position": [
        -384,
        272
      ],
      "webhookId": "6da9b6a6-4cae-4c9f-98eb-88ca508147ae",
      "credentials": {
        "gmailOAuth2": {
          "id": "U4PCI0t9RptOHFrU",
          "name": "Gmail account"
        }
      }
    },
    {
      "parameters": {
        "authentication": "airtableOAuth2Api",
        "operation": "update",
        "base": {
          "__rl": true,
          "value": "appkFksjr82WRknGp",
          "mode": "list",
          "cachedResultName": "Untitled Base"
        },
        "table": {
          "__rl": true,
          "value": "tblC1UIxsAoJbuPQF",
          "mode": "list",
          "cachedResultName": "Leads"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "id": "={{ $('ComposeFinalReengagement').item.json.record_id }}",
            "Status": "={{ $('ComposeFinalReengagement').item.json.new_status }}",
            "Follow-up Count": "={{ ($('ComposeFinalReengagement').item.json.fields['Follow-up Count'] || 0) + 1 }}",
            "Last Contacted": "={{ $now }}"
          },
          "matchingColumns": [
            "id"
          ],
          "schema": []
        },
        "options": {}
      },
      "id": "5eee8e45-c76f-4409-8530-cb252326f495",
      "name": "UpdateFinalTracking",
      "type": "n8n-nodes-base.airtable",
      "typeVersion": 2.2,
      "position": [
        -144,
        272
      ],
      "credentials": {
        "airtableOAuth2Api": {
          "id": "XJFBTNiaiIQT42jz",
          "name": "Airtable account 3"
        }
      }
    },
    {
      "parameters": {
        "content": "Booking Handler \n",
        "height": 272,
        "width": 2272,
        "color": 5
      },
      "id": "926c6247-12a2-40fc-a592-a6a2f1b40069",
      "name": "Sticky Note2",
      "type": "n8n-nodes-base.stickyNote",
      "typeVersion": 1,
      "position": [
        -2064,
        528
      ]
    },
    {
      "parameters": {
        "authentication": "oAuth2",
        "select": "channel",
        "channelId": {
          "__rl": true,
          "mode": "list",
          "value": ""
        },
        "otherOptions": {}
      },
      "id": "46d0c2f1-dcc0-429f-8b07-b21709bb0e82",
      "name": "Send a message",
      "type": "n8n-nodes-base.slack",
      "typeVersion": 2.7,
      "position": [
        -960,
        560
      ],
      "webhookId": "a1631085-b175-4ed6-9233-2a83a83a344c",
      "credentials": {
        "slackOAuth2Api": {
          "id": "gM0EJZcnajZWSpvd",
          "name": "Slack account 2"
        }
      },
      "disabled": true
    },
    {
      "parameters": {
        "pollTimes": {
          "item": [
            {
              "mode": "everyMinute"
            }
          ]
        },
        "simple": false,
        "filters": {
          "readStatus": "unread"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.gmailTrigger",
      "typeVersion": 1.2,
      "position": [
        -2048,
        928
      ],
      "id": "096493c2-e088-4ec2-a753-38f567151540",
      "name": "ReplyTrigger",
      "credentials": {
        "gmailOAuth2": {
          "id": "U4PCI0t9RptOHFrU",
          "name": "Gmail account"
        }
      }
    },
    {
      "parameters": {
        "authentication": "airtableOAuth2Api",
        "operation": "search",
        "base": {
          "__rl": true,
          "value": "appkFksjr82WRknGp",
          "mode": "list",
          "cachedResultName": "Untitled Base"
        },
        "table": {
          "__rl": true,
          "value": "tblC1UIxsAoJbuPQF",
          "mode": "list",
          "cachedResultName": "Leads"
        },
        "filterByFormula": "={{ '{Email} = \"' + $json.from.value[0].address + '\"' }}",
        "options": {}
      },
      "type": "n8n-nodes-base.airtable",
      "typeVersion": 2.2,
      "position": [
        -1808,
        928
      ],
      "id": "b706caa6-024a-4029-b634-babc2968a0d6",
      "name": "MatchLeadByEmail",
      "credentials": {
        "airtableOAuth2Api": {
          "id": "XJFBTNiaiIQT42jz",
          "name": "Airtable account 3"
        }
      }
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "cond-1",
              "leftValue": "={{ $json.id }}",
              "rightValue": "",
              "operator": {
                "type": "string",
                "operation": "notEmpty",
                "singleValue": true
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        -1568,
        928
      ],
      "id": "2488ab86-eec9-429b-9e90-b272721a198f",
      "name": "IsKnownLead"
    },
    {
      "parameters": {
        "modelId": {
          "__rl": true,
          "value": "chat-latest",
          "mode": "list",
          "cachedResultName": "CHAT-LATEST"
        },
        "responses": {
          "values": [
            {
              "role": "system",
              "content": "You are a helpful assistant for EasyMarketPro Limited, an AI automation agency.\n\nA lead has replied to a follow-up email with a question. Answer their question using only the information in the knowledge base below. Keep the tone warm and consultative, not salesy. Keep the reply short - a few sentences, not an essay.\n\nIf the question cannot be answered from this information, do not guess. Instead say you'll have someone from the team follow up personally with the specific details.\n\nKNOWLEDGE BASE:\n[PASTE YOUR FAQ / KNOWLEDGE BASE CONTENT HERE - pricing ranges, what EasyMarketPro does, typical timelines, tools used, engagement process, etc.]"
            },
            {
              "content": "=Lead's name: {{ $('MatchLeadByEmail').item.json.fields.Name }}\nLead's company: {{ $('MatchLeadByEmail').item.json.fields.Company }}\nLead's original pain point: {{ $('MatchLeadByEmail').item.json.fields['Pain Point Summary'] }}\n\nLead's reply/question: {{ $('ReplyTrigger').item.json.text }}"
            }
          ]
        },
        "builtInTools": {},
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.openAi",
      "typeVersion": 2.3,
      "position": [
        -1328,
        848
      ],
      "id": "05522eba-b44b-4746-a77e-1a79348e258e",
      "name": "KnowledgeBaseAssistant",
      "credentials": {
        "openAiApi": {
          "id": "Iuj3PecT9isReL1E",
          "name": "OpenAI account"
        }
      }
    },
    {
      "parameters": {
        "mode": "runOnceForEachItem",
        "jsCode": "const raw = $json.output[0].content[0].text.message;\n\nlet replyText;\ntry {\n  const parsed = JSON.parse(raw);\n  replyText = typeof parsed === 'string' ? parsed : raw;\n} catch (e) {\n  replyText = raw;\n}\n\nreturn {\n  ...$json,\n  reply_text: replyText\n};"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1040,
        848
      ],
      "id": "7a101c72-b4cc-4488-af21-b6527393d3af",
      "name": "ExtractReplyText"
    },
    {
      "parameters": {
        "operation": "reply",
        "messageId": "={{ $('ReplyTrigger').item.json.id }}",
        "message": "={{ $json.reply_text }}",
        "options": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.1,
      "position": [
        -848,
        848
      ],
      "id": "73b73c70-0671-4a77-beda-3876b2c816a3",
      "name": "SendReply",
      "webhookId": "52553d52-b1c4-41c4-8d96-4dc072c127b3",
      "credentials": {
        "gmailOAuth2": {
          "id": "U4PCI0t9RptOHFrU",
          "name": "Gmail account"
        }
      }
    },
    {
      "parameters": {
        "authentication": "airtableOAuth2Api",
        "operation": "update",
        "base": {
          "__rl": true,
          "value": "appkFksjr82WRknGp",
          "mode": "list",
          "cachedResultName": "Untitled Base"
        },
        "table": {
          "__rl": true,
          "value": "tblC1UIxsAoJbuPQF",
          "mode": "list",
          "cachedResultName": "Leads"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "Last Contacted": "={{ $now }}"
          },
          "matchingColumns": [
            "id"
          ],
          "schema": []
        },
        "options": {}
      },
      "type": "n8n-nodes-base.airtable",
      "typeVersion": 2.2,
      "position": [
        -608,
        848
      ],
      "id": "51ae5e64-5113-4c08-8077-78966cf25cf2",
      "name": "LogReplyHandled",
      "credentials": {
        "airtableOAuth2Api": {
          "id": "XJFBTNiaiIQT42jz",
          "name": "Airtable account 3"
        }
      }
    },
    {
      "parameters": {
        "content": "REPLY ASSISTANT",
        "height": 368,
        "width": 2304,
        "color": 3
      },
      "type": "n8n-nodes-base.stickyNote",
      "position": [
        -2080,
        784
      ],
      "typeVersion": 1,
      "id": "7766a58a-c15e-4be7-9d98-7276d5e72741",
      "name": "Sticky Note3"
    }
  ],
  "connections": {
    "LeadCapture": {
      "main": [
        [
          {
            "node": "LeadExtraction",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "LeadExtraction": {
      "main": [
        [
          {
            "node": "LeadQualification",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "LeadQualification": {
      "main": [
        [
          {
            "node": "QualificationParse",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "QualificationParse": {
      "main": [
        [
          {
            "node": "If",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "If": {
      "main": [
        [
          {
            "node": "LeadScoring",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "DisqualifiedLeads",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "LeadScoring": {
      "main": [
        [
          {
            "node": "QualifiedLeads",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "QualifiedLeads": {
      "main": [
        [
          {
            "node": "PriorityRouter",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "PriorityRouter": {
      "main": [
        [
          {
            "node": "ComposeHotMessage",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "ComposeNurtureMessage",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ComposeHotMessage": {
      "main": [
        [
          {
            "node": "MessageHotLead",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "MessageHotLead": {
      "main": [
        [
          {
            "node": "UpdateHotLeads",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ComposeNurtureMessage": {
      "main": [
        [
          {
            "node": "MessageNurtureLead",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "MessageNurtureLead": {
      "main": [
        [
          {
            "node": "UpdateNurtureLead",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Check Google calender": {
      "main": [
        [
          {
            "node": "GetBookings",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "GetBookings": {
      "main": [
        [
          {
            "node": "ExtractInvitee",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ExtractInvitee": {
      "main": [
        [
          {
            "node": "FindBookedLead",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "FindBookedLead": {
      "main": [
        [
          {
            "node": "IsNotBookedYet",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "IsNotBookedYet": {
      "main": [
        [
          {
            "node": "UpdateStatusBooked",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "UpdateStatusBooked": {
      "main": [
        [
          {
            "node": "Send a message",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ReEngagementSchedule": {
      "main": [
        [
          {
            "node": "QueryFollowUpLeads1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "QueryFollowUpLeads1": {
      "main": [
        [
          {
            "node": "LoopLeads1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "DetermineStage1": {
      "main": [
        [
          {
            "node": "ShouldSend",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ShouldSend": {
      "main": [
        [
          {
            "node": "RouteStage",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "LoopLeads1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "RouteStage": {
      "main": [
        [
          {
            "node": "ComposeHotFollowup",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "ComposeNurtureFollowup",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "ComposeFinalReengagement",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ComposeHotFollowup": {
      "main": [
        [
          {
            "node": "GmailHotFollowup",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "GmailHotFollowup": {
      "main": [
        [
          {
            "node": "UpdateHotTracking",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "UpdateHotTracking": {
      "main": [
        [
          {
            "node": "LoopLeads1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "LoopLeads1": {
      "main": [
        [],
        [
          {
            "node": "DetermineStage1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ComposeNurtureFollowup": {
      "main": [
        [
          {
            "node": "GmailNurtureFollowup",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "GmailNurtureFollowup": {
      "main": [
        [
          {
            "node": "UpdateNurtureTracking",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "UpdateNurtureTracking": {
      "main": [
        [
          {
            "node": "LoopLeads1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ComposeFinalReengagement": {
      "main": [
        [
          {
            "node": "GmailFinalReengagement",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "GmailFinalReengagement": {
      "main": [
        [
          {
            "node": "UpdateFinalTracking",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "UpdateFinalTracking": {
      "main": [
        [
          {
            "node": "LoopLeads1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ReplyTrigger": {
      "main": [
        [
          {
            "node": "MatchLeadByEmail",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "MatchLeadByEmail": {
      "main": [
        [
          {
            "node": "IsKnownLead",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "IsKnownLead": {
      "main": [
        [
          {
            "node": "KnowledgeBaseAssistant",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "KnowledgeBaseAssistant": {
      "main": [
        [
          {
            "node": "ExtractReplyText",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ExtractReplyText": {
      "main": [
        [
          {
            "node": "SendReply",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "SendReply": {
      "main": [
        [
          {
            "node": "LogReplyHandled",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "settings": {
    "executionOrder": "v1",
    "binaryMode": "separate",
    "availableInMCP": false
  },
  "staticData": {
    "node:Check Google calender": {
      "recurrenceRules": [
        null
      ],
      "recurrenceRuleSignatures": [
        null
      ]
    },
    "node:ReEngagementSchedule": {
      "recurrenceRules": [
        null
      ],
      "recurrenceRuleSignatures": [
        null
      ]
    },
    "node:LeadCapture": {
      "documentId": "1bEJAU8RsP7pjRvrXF7D6tuxRFv2h7Xn7HBKXf8qC7qM",
      "sheetId": 1889259625,
      "lastIndexChecked": 2
    }
  },
  "pinData": {},
  "triggerCount": 3,
  "meta": {
    "templateCredsSetupCompleted": true
  }
}
