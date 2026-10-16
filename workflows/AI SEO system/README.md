{
  "nodes": [
    {
      "parameters": {
        "rule": {
          "interval": [
            {
              "field": "weeks",
              "triggerAtHour": 12
            }
          ]
        }
      },
      "type": "n8n-nodes-base.scheduleTrigger",
      "typeVersion": 1.4,
      "position": [
        -1664,
        -512
      ],
      "id": "396ec101-2ae4-4351-9b78-992c5cb24410",
      "name": "WeeklyTrigger"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://api.dataforseo.com/v3/dataforseo_labs/google/related_keywords/live",
        "authentication": "genericCredentialType",
        "genericAuthType": "httpBasicAuth",
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "=[\n  {\n    \"keyword\": \"AI automation\",\n    \"location_code\": 2840,\n    \"language_code\": \"en\",\n    \"depth\": 3,\n    \"limit\": 100,\n    \"include_seed_keyword\": true\n  }\n]",
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.5,
      "position": [
        -1536,
        -512
      ],
      "id": "93e85d00-d8a8-4bc1-b680-c426f9d27ea6",
      "name": "GetKeywords",
      "credentials": {
        "httpBasicAuth": {
          "id": "A0HdyeiCt5yD5Rfm",
          "name": "DataForSEO API"
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
              "id": "8d2a682c-94c3-47f9-949a-3c6a247cadea",
              "leftValue": "={{ $json.volume }}",
              "rightValue": 50,
              "operator": {
                "type": "number",
                "operation": "gte"
              }
            },
            {
              "id": "65479a3f-30a2-4027-a58c-1030b751bc7a",
              "leftValue": "={{ $json.difficulty }}",
              "rightValue": "",
              "operator": {
                "type": "number",
                "operation": "notEmpty",
                "singleValue": true
              }
            },
            {
              "id": "826fb3b3-5db7-46dd-81cd-1c897315501f",
              "leftValue": "={{ $json.difficulty }}",
              "rightValue": 40,
              "operator": {
                "type": "number",
                "operation": "lte"
              }
            },
            {
              "id": "d2167802-76f6-412f-85b0-e41c9b0ab680",
              "leftValue": "={{ ['informational','commercial'].includes($json.intent) }}",
              "rightValue": "",
              "operator": {
                "type": "boolean",
                "operation": "true",
                "singleValue": true
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.filter",
      "typeVersion": 2.3,
      "position": [
        -1232,
        -512
      ],
      "id": "08b7d2f7-80f1-44dd-88cc-4f29371bb3bd",
      "name": "FilterKeywords"
    },
    {
      "parameters": {
        "jsCode": "const rows = $input.all().map(i => i.json);\nreturn [{\n  json: {\n    list: rows.map(r => `${r.keyword} | vol ${r.volume} | ${r.intent}`).join('\\n')\n  }\n}];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1072,
        -512
      ],
      "id": "946294df-6596-4c83-936f-1f8a809a3da2",
      "name": "CombineKeywords"
    },
    {
      "parameters": {
        "modelId": {
          "__rl": true,
          "value": "gpt-5",
          "mode": "list",
          "cachedResultName": "GPT-5"
        },
        "responses": {
          "values": [
            {
              "content": "=You filter keyword ideas for the blog of a B2B AI automation agency. The target reader is a business owner or operations leader who is evaluating or planning to implement AI automation in their company to save time or cost. Keep a keyword only if that person would plausibly search it.\n\nRemove keywords whose searcher is a learner or job seeker (courses, certificates, beginner tutorials, jobs), a developer (code, github, open source setup), a consumer or hobbyist (chatbots with no restrictions, free AI app lists, top AI apps, roleplay), or someone looking for forum threads (reddit). Remove brand navigation queries.\n\nAlso remove keywords that ask for free tools, general consumer productivity or tool roundups unrelated to business automation, and single broad head terms such as \"ai automation\".\n\nReturn only JSON in this exact shape: {\"keep\": [\"keyword one\", \"keyword two\"]}\nUse each keyword exactly as written.\n\nKeywords to judge:\n{{ $json.list }}"
            }
          ]
        },
        "builtInTools": {},
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.openAi",
      "typeVersion": 2.3,
      "position": [
        -912,
        -512
      ],
      "id": "01dae76a-bcfe-424a-b5c9-e84fd4266ae2",
      "name": "JudgeRelevance",
      "credentials": {
        "openAiApi": {
          "id": null,
          "name": "",
          "__aiGatewayManaged": true
        }
      }
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk",
          "mode": "list",
          "cachedResultName": "Keywords",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Sheet1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk/edit#gid=0"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -448,
        -512
      ],
      "id": "90db884b-e90d-4a37-adc1-4aaf75bb76bf",
      "name": "GetExistingKeywords",
      "alwaysOutputData": true,
      "executeOnce": true,
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "BJAdPdxFDQWm8ouf",
          "name": "Google Sheets account 2"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "const existing = new Set(\n  $('GetExistingKeywords').all()\n    .map(i => (i.json.keyword || '').toLowerCase().trim())\n    .filter(Boolean)\n);\n\nreturn $('ScoreKeywords').all()\n  .filter(i => !existing.has(i.json.keyword.toLowerCase().trim()))\n  .map(i => ({ json: i.json }));"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -256,
        -512
      ],
      "id": "d0378219-d899-4b5d-9f20-5c3f3225dd8b",
      "name": "CheckDuplicateKeywords"
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk",
          "mode": "list",
          "cachedResultName": "Keywords",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Sheet1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk/edit#gid=0"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "keyword": "={{ $json.keyword }}",
            "volume": "={{ $json.volume }}",
            "difficulty": "={{ $json.difficulty }}",
            "intent": "={{ $json.intent }}",
            "cpc": "={{ $json.cpc }}",
            "has_paa": "={{ $json.has_paa }}",
            "score": "={{ $json.score }}",
            "scheduled_day": "={{ $json.scheduled_day }}",
            "status": "={{ $json.status }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "keyword",
              "displayName": "keyword",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "volume",
              "displayName": "volume",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "difficulty",
              "displayName": "difficulty",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "intent",
              "displayName": "intent",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "cpc",
              "displayName": "cpc",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "has_paa",
              "displayName": "has_paa",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "score",
              "displayName": "score",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "scheduled_day",
              "displayName": "scheduled_day",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "status",
              "displayName": "status",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        320,
        -512
      ],
      "id": "049553e6-6a25-459b-96a2-f0e109652b34",
      "name": "SaveKeywords",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "BJAdPdxFDQWm8ouf",
          "name": "Google Sheets account 2"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "const items = $input.first().json.tasks?.[0]?.result?.[0]?.items ?? [];\n\nreturn items.map(i => ({\n  json: {\n    keyword: i.keyword_data.keyword,\n    volume: i.keyword_data.keyword_info?.search_volume ?? 0,\n    difficulty: i.keyword_data.keyword_properties?.keyword_difficulty ?? null,\n    intent: i.keyword_data.search_intent_info?.main_intent ?? null,\n    cpc: i.keyword_data.keyword_info?.cpc ?? null,\n    has_paa: i.keyword_data.serp_info?.serp_item_types?.includes('people_also_ask') ?? false,\n    related_keywords: i.related_keywords ?? []\n  }\n}));"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1392,
        -512
      ],
      "id": "eaff3453-94a2-4c2b-94e4-f9647fba55a9",
      "name": "SplitKeywords"
    },
    {
      "parameters": {
        "jsCode": "const raw = $input.first().json.output[0].content[0].text\n  .replace(/```json|```/g, '')\n  .trim();\nconst keep = JSON.parse(raw).keep.map(k => k.toLowerCase().trim());\n\nconst rows = $('FilterKeywords').all().map(i => i.json);\n\nreturn rows\n  .filter(r => keep.includes(r.keyword.toLowerCase().trim()))\n  .map(r => ({\n    json: {\n      ...r,\n      score: r.volume / Math.pow(r.difficulty + 1, 2)\n    }\n  }));"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -624,
        -512
      ],
      "id": "1b6033c5-542f-46d8-bb0c-5fcb538f84a0",
      "name": "ScoreKeywords"
    },
    {
      "parameters": {
        "jsCode": "const days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday'];\n\nconst rows = $input.all().map(i => i.json)\n  .sort((a, b) => b.score - a.score)\n  .slice(0, 5);\n\nreturn rows.map((r, i) => ({\n  json: {\n    ...r,\n    scheduled_day: days[i],\n    status: r.source === 'related_backfill' ? 'pending_draft_needs_data' : 'pending_draft'\n  }\n}));"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        128,
        -512
      ],
      "id": "298db216-a110-4361-b8b0-b49482d7fe4d",
      "name": "RankAndCap"
    },
    {
      "parameters": {
        "jsCode": "const current = $input.all().map(i => i.json);\nconst needed = 5 - current.length;\n\nif (needed <= 0) return current.map(r => ({ json: r }));\n\nconst existing = new Set(\n  $('GetExistingKeywords').all()\n    .map(i => (i.json.keyword || '').toLowerCase().trim())\n);\nconst usedThisRun = new Set(current.map(r => r.keyword.toLowerCase().trim()));\n\nconst allFiltered = $('FilterKeywords').all().map(i => i.json);\nconst candidatePool = new Map();\n\nallFiltered.forEach(r => {\n  (r.related_keywords || []).forEach(rk => {\n    const key = rk.toLowerCase().trim();\n    if (!existing.has(key) && !usedThisRun.has(key) && !candidatePool.has(key)) {\n      candidatePool.set(key, {\n        keyword: rk, volume: null, difficulty: null, intent: null,\n        cpc: null, has_paa: false, score: 0, source: 'related_backfill'\n      });\n    }\n  });\n});\n\nconst topUp = Array.from(candidatePool.values()).slice(0, needed);\n\nreturn [...current, ...topUp].map(r => ({ json: r }));// Loop over input items and add a new field called 'myNewField' to the "
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -48,
        -512
      ],
      "id": "2b3c810f-7839-4550-9c14-a3aa129d6645",
      "name": "TopUpFromRelated"
    },
    {
      "parameters": {
        "content": "## KEWORD RESEARCH & BACKLINK RESEARCH\nRuns automatically once a week.\n\nPull data — Searches Google for keywords using a broad seed and gets ~90 results with search volume, difficulty, and buyer intent.\nFilter — Removes keywords with too little search volume, too much competition, or the wrong intent.\nCheck relevance — AI reviews the list and keeps only keywords a real business buyer would search (drops student, developer, and hobbyist searches).\nScore — Ranks keywords by how realistic they are to rank for right now, favoring easier wins over pure search volume.\nRemove duplicates — Checks against past weeks so we never repeat a keyword already used.\nFill gaps — If too few new keywords are found, pulls fresh related terms instead of reusing old ones.\nFinalize — Picks the top 5, assigns one to each weekday (Monday-Friday), and saves them to the spreadsheet.\n\n",
        "height": 384,
        "width": 560
      },
      "type": "n8n-nodes-base.stickyNote",
      "position": [
        -2224,
        -672
      ],
      "typeVersion": 1,
      "id": "acc71ac7-d665-473f-914d-2767365beb3e",
      "name": "Sticky Note"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://api.dataforseo.com/v3/serp/google/organic/live/advanced",
        "authentication": "genericCredentialType",
        "genericAuthType": "httpBasicAuth",
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "=[\n  {\n    \"keyword\": \"{{ $json.keyword }}\",\n    \"location_code\": 2840,\n    \"language_code\": \"en\",\n    \"device\": \"desktop\",\n    \"depth\": 10\n  }\n]",
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.5,
      "position": [
        -1584,
        -272
      ],
      "id": "54e2fcb7-ebd9-4b40-a262-2bd132f2095f",
      "name": "GetSERP",
      "credentials": {
        "httpBasicAuth": {
          "id": "A0HdyeiCt5yD5Rfm",
          "name": "DataForSEO API"
        }
      }
    },
    {
      "parameters": {
        "mode": "runOnceForEachItem",
        "jsCode": "const data = $json.tasks?.[0]?.result?.[0];\nconst items = data?.items ?? [];\n\nconst excludedDomains = ['reddit.com', 'youtube.com'];\n\nconst organicResults = items\n  .filter(i => i.type === 'organic')\n  .filter(i => !excludedDomains.some(d => (i.domain || '').includes(d)))\n  .slice(0, 5)\n  .map(i => ({\n    title: i.title,\n    url: i.url,\n    domain: i.domain,\n    description: i.description\n  }));\n\nconst paaQuestions = items\n  .filter(i => i.type === 'people_also_ask')\n  .flatMap(i => i.items || [])\n  .map(q => q.title);\n\nreturn {\n  json: {\n    keyword: data?.keyword,\n    top_organic_results: organicResults,\n    paa_questions: paaQuestions\n  }\n};"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1440,
        -272
      ],
      "id": "8183cf94-6801-4bf1-8ffa-c58f2b9af8ca",
      "name": "ParseSERP"
    },
    {
      "parameters": {
        "fieldToSplitOut": "top_organic_results",
        "include": "allOtherFields",
        "options": {}
      },
      "type": "n8n-nodes-base.splitOut",
      "typeVersion": 1,
      "position": [
        -1264,
        -272
      ],
      "id": "bd0d8037-4d60-4291-b198-a3e10c2bd21a",
      "name": "SplitByResult"
    },
    {
      "parameters": {
        "jsCode": "const items = $input.all();\nconst grouped = {};\n\nitems.forEach(item => {\n  const kw = item.json.keyword;\n  if (!grouped[kw]) grouped[kw] = [];\n  if (grouped[kw].length < 3) grouped[kw].push(item);\n});\n\nreturn Object.values(grouped).flat();"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1104,
        -272
      ],
      "id": "974e0404-62e9-4c6e-a38d-c38d6b1cd5b1",
      "name": "TrimToTop3"
    },
    {
      "parameters": {
        "url": "={{ $json.top_organic_results.url }}",
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.5,
      "position": [
        -944,
        -272
      ],
      "id": "87381fcd-c39a-42ba-81e4-bdadc5a6b2d5",
      "name": "FetchCompetitorPages",
      "alwaysOutputData": true,
      "onError": "continueErrorOutput"
    },
    {
      "parameters": {
        "operation": "extractHtmlContent",
        "extractionValues": {
          "values": [
            {
              "key": "body_text",
              "cssSelector": "body"
            }
          ]
        },
        "options": {
          "trimValues": false
        }
      },
      "type": "n8n-nodes-base.html",
      "typeVersion": 1.2,
      "position": [
        -752,
        -336
      ],
      "id": "f6eb8b40-305b-4bfe-ab69-e50cd7151b3d",
      "name": "ExtractArticleText"
    },
    {
      "parameters": {
        "jsCode": "return $input.all().map(item => ({\n  json: {\n    keyword: item.json.keyword,\n    url: item.json.top_organic_results?.url || item.json.url,\n    title: item.json.top_organic_results?.title || null,\n    description: item.json.top_organic_results?.description || null,\n    body_text: null,\n    fetch_failed: true\n  }\n}));"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -720,
        -128
      ],
      "id": "731d3059-3246-44a2-abf2-4000992c9457",
      "name": "HandleFetchErrors",
      "alwaysOutputData": true
    },
    {
      "parameters": {
        "jsCode": "return $input.all().map(item => {\n  const text = item.json.body_text || '';\n  return {\n    json: {\n      ...item.json,\n      body_text: text.slice(0, 6000)\n    }\n  };\n});"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -272,
        -192
      ],
      "id": "04f6aca6-f5b9-4470-9ef1-6484d718fe3e",
      "name": "TrimBodyText"
    },
    {
      "parameters": {
        "jsCode": "const items = $input.all();\nconst grouped = {};\n\nitems.forEach(item => {\n  const kw = item.json.keyword;\n  if (!grouped[kw]) grouped[kw] = [];\n  grouped[kw].push(item.json);\n});\n\nreturn Object.entries(grouped).map(([keyword, pages]) => ({\n  json: {\n    keyword,\n    pages\n  }\n}));"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -112,
        -192
      ],
      "id": "65dbe204-4cf9-4aea-8659-866d4392e13c",
      "name": "GroupByKeyword"
    },
    {
      "parameters": {
        "jsCode": "const extracted = $input.all();\nconst original = $('TrimToTop3').all();\n\nreturn extracted.map((item, i) => ({\n  json: {\n    keyword: original[i]?.json?.keyword,\n    url: original[i]?.json?.top_organic_results?.url || original[i]?.json?.url,\n    title: original[i]?.json?.top_organic_results?.title,\n    body_text: item.json.body_text\n  }\n}));"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -576,
        -336
      ],
      "id": "7db3640a-fdeb-483b-b9f8-74e42a2d2719",
      "name": "RestoreKeywordFields"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.merge",
      "typeVersion": 3.2,
      "position": [
        -432,
        -192
      ],
      "id": "1e6fcec3-11f0-483f-b666-6d01abbb9779",
      "name": "Merge"
    },
    {
      "parameters": {
        "modelId": {
          "__rl": true,
          "value": "gpt-4.1",
          "mode": "list",
          "cachedResultName": "GPT-4.1"
        },
        "responses": {
          "values": [
            {
              "content": "=You are building a content brief for a B2B blog post targeting the keyword: {{ $json.keyword }}\n\nBelow are the top 3 pages currently ranking for this keyword. Read them and extract what the post needs to cover to compete.\n\n{{ $json.pages.map((p, i) => `COMPETITOR ${i + 1}: ${p.title}\\nURL: ${p.url}\\n${p.body_text}`).join('\\n\\n---\\n\\n') }}\n\nReturn only JSON in this exact shape:\n{\n  \"shared_subtopics\": [\"topic every competitor covers, so the post must include it\"],\n  \"content_gaps\": [\"something none of the competitors cover well, a real opportunity\"],\n  \"recommended_headings\": [\"a suggested H2/H3 heading for the post, in order\"],\n  \"format_note\": \"one sentence on whether competitors are blog posts, product pages, or something else, and what that means for whether this keyword suits a blog post\"\n}"
            }
          ]
        },
        "builtInTools": {},
        "options": {
          "textFormat": {
            "textOptions": {
              "type": "json_object"
            }
          }
        }
      },
      "type": "@n8n/n8n-nodes-langchain.openAi",
      "typeVersion": 2.3,
      "position": [
        32,
        -192
      ],
      "id": "6dd24e44-fab4-4495-b3d6-8f7428ad3a6a",
      "name": "CompetitorBrief",
      "credentials": {
        "openAiApi": {
          "id": null,
          "name": "",
          "__aiGatewayManaged": true
        }
      }
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "1hgJGLmZGoOYLAj8aAMJtWskhzQZuQTkzcjIJrSGXtQ8",
          "mode": "list",
          "cachedResultName": "Briefs",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1hgJGLmZGoOYLAj8aAMJtWskhzQZuQTkzcjIJrSGXtQ8/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Sheet1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1hgJGLmZGoOYLAj8aAMJtWskhzQZuQTkzcjIJrSGXtQ8/edit#gid=0"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "keyword": "={{ $('GroupByKeyword').item.json.keyword }}",
            "shared_subtopics": "={{ $json.output[0].content[0].text.shared_subtopics.join('; ') }}",
            "recommended_headings": "={{ $json.output[0].content[0].text.recommended_headings.join('; ') }}",
            "content_gaps": "={{ $json.output[0].content[0].text.content_gaps.join('; ') }}",
            "format_note": "={{ $json.output[0].content[0].text.format_note }}",
            "date_added": "={{ $now.toISODate() }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "keyword",
              "displayName": "keyword",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "shared_subtopics",
              "displayName": "shared_subtopics",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "content_gaps",
              "displayName": "content_gaps",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "recommended_headings",
              "displayName": "recommended_headings",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "format_note",
              "displayName": "format_note",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "date_added",
              "displayName": "date_added",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        320,
        -192
      ],
      "id": "bdbb11e1-0e29-4a7b-8d60-73abf24da4ac",
      "name": "SaveBrief",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "BJAdPdxFDQWm8ouf",
          "name": "Google Sheets account 2"
        }
      }
    },
    {
      "parameters": {
        "content": "## COMPETITOR ANALYSIS \nRuns after the 5 keywords are ranked and saved. For each keyword, researches what's already ranking on Google and extracts a content brief.\n\nFlow: GetSERP (pulls top 10 Google results per keyword) → ParseSERP (keeps organic results, drops Reddit/YouTube/ads, captures People Also Ask) → SplitByResult → TrimToTop3 (keeps only the top 3 competitor pages per keyword) → FetchCompetitorPages (downloads each page, errors routed to HandleFetchErrors as a fallback) → ExtractArticleText (pulls readable text from the page HTML) → RestoreKeywordFields (re-attaches keyword/url/title lost during extraction) → Merge (combines successful fetches with fallback data) → TrimBodyText (caps each page's text to stay within AI token limits) → GroupByKeyword (groups the 3 pages back under their keyword) → CompetitorBrief (AI reads the 3 competitor pages and outputs shared subtopics, content gaps, and recommended headings) → SaveBrief (writes the brief to the sheet).",
        "height": 288,
        "width": 576,
        "color": 4
      },
      "type": "n8n-nodes-base.stickyNote",
      "position": [
        -2224,
        -304
      ],
      "typeVersion": 1,
      "id": "9981b224-1836-467c-907a-af7244cd47a7",
      "name": "Sticky Note1"
    },
    {
      "parameters": {
        "modelId": {
          "__rl": true,
          "value": "gpt-4.1",
          "mode": "list",
          "cachedResultName": "GPT-4.1"
        },
        "responses": {
          "values": [
            {
              "content": "=Write a B2B blog post for an AI automation agency, targeting the keyword: {{ $('GroupByKeyword').item.json.keyword }}\n\nUse this structure, based on competitor research:\nRecommended headings: {{ $json.output[0].content[0].text.recommended_headings.join(', ') }}\n\nCover these subtopics, since competitors all include them and skipping them would hurt rankings:\n{{ $json.output[0].content[0].text.shared_subtopics.join(', ') }}\n\nAddress these gaps, since none of the top-ranking competitors cover them well, this is where the post can stand out:\n{{ $json.output[0].content[0].text.content_gaps.join(', ') }}\n\nWrite 1200 to 1800 words in markdown, with H2 and H3 headings matching the recommended structure. Voice: confident, direct, practical B2B tone, no filler, no hedging language, written for a business owner or operations leader evaluating AI automation, not a developer. Avoid generic AI-written phrasing like \"In today's fast-paced world\" or \"it's important to note.\" End with a short, non-pushy call to action inviting the reader to explore AI automation for their business.\n\nReturn only the markdown blog post, no commentary before or after it."
            }
          ]
        },
        "builtInTools": {},
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.openAi",
      "typeVersion": 2.3,
      "position": [
        -1520,
        0
      ],
      "id": "559db007-bb3c-4203-ab89-5a9b147a806b",
      "name": "DraftBlogPost",
      "credentials": {
        "openAiApi": {
          "id": null,
          "name": "",
          "__aiGatewayManaged": true
        }
      }
    },
    {
      "parameters": {
        "content": "## CONTENT CREATION & DISTRIBUTION\nRuns after the competitor brief is saved. Drafts a blog post per keyword, holds it for editor approval, then publishes and repurposes it once approved.\n\nFlow: DraftBlogPost (GPT-4.1 writes a 1200-1800 word B2B post from the brief's headings, subtopics, and gaps) → MergeKeywordRow (re-attaches keyword row, draft, and headings) → UpdateDraftStatus (saves draft to the sheet as pending_approval) → DailyPostTrigger (9am Mon-Fri) → GetTodayPost (pulls the row scheduled for today with pending_approval status) → SendApprovalEmail (emails the draft and waits for the editor's free-text reply) → CheckApproval (approved goes to the True branch, anything else to the False branch). If edits are requested: ReviseDraft (Gemini rewrites the draft using the editor's notes) → Edit Fields (puts the revision into draft_content) → back to SendApprovalEmail. If approved, three parallel branches: PublishWebsite (posts keyword and draft to the site endpoint) → UpdateStatus (marks the row Published); RepurposeLinkedIn (Gemini condenses the draft into a LinkedIn post); RepurposeNewsletter (Gemini condenses the draft into a newsletter section) → PublishNewsletter (emails it via Gmail).",
        "height": 432,
        "width": 576,
        "color": 3
      },
      "type": "n8n-nodes-base.stickyNote",
      "position": [
        -2224,
        -16
      ],
      "typeVersion": 1,
      "id": "547b3513-b0fb-44ee-b753-73ade7de07f8",
      "name": "Sticky Note2"
    },
    {
      "parameters": {
        "jsCode": "return $input.all().map((item, i) => {\n  const keywordRow = $('RankAndCap').all()[i]?.json || {};\n  const draftText = item.json.output?.[0]?.content?.[0]?.text || '';\n  const headings = $('CompetitorBrief').all()[i]?.json?.output?.[0]?.content?.[0]?.text?.recommended_headings?.join('; ') || '';\n\n  return {\n    json: {\n      ...keywordRow,\n      draft_content: draftText,\n      recommended_headings: headings\n    }\n  };\n});"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1200,
        0
      ],
      "id": "af2ada65-7cfd-4eb4-b333-06cadfb18454",
      "name": "MergeKeywordRow"
    },
    {
      "parameters": {
        "operation": "update",
        "documentId": {
          "__rl": true,
          "value": "1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk",
          "mode": "list",
          "cachedResultName": "Keywords",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Sheet1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk/edit#gid=0"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "keyword": "={{ $json.keyword }}",
            "draft_content": "={{ $json.draft_content }}",
            "recommended_headings": "={{ $json.recommended_headings }}",
            "status": "pending_approval"
          },
          "matchingColumns": [
            "keyword"
          ],
          "schema": [
            {
              "id": "keyword",
              "displayName": "keyword",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "volume",
              "displayName": "volume",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "difficulty",
              "displayName": "difficulty",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "intent",
              "displayName": "intent",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "cpc",
              "displayName": "cpc",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "has_paa",
              "displayName": "has_paa",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "related_keywords",
              "displayName": "related_keywords",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "score",
              "displayName": "score",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "scheduled_day",
              "displayName": "scheduled_day",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "status",
              "displayName": "status",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "draft_content",
              "displayName": "draft_content",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "recommended_headings",
              "displayName": "recommended_headings",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -992,
        0
      ],
      "id": "4315509b-2628-43e0-a0ee-64c7603f3d44",
      "name": "UpdateDraftStatus",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "BJAdPdxFDQWm8ouf",
          "name": "Google Sheets account 2"
        }
      }
    },
    {
      "parameters": {
        "rule": {
          "interval": [
            {
              "field": "cronExpression",
              "expression": "0 9 * * 1-5"
            }
          ]
        }
      },
      "type": "n8n-nodes-base.scheduleTrigger",
      "typeVersion": 1.4,
      "position": [
        -1472,
        176
      ],
      "id": "4bc58580-5a97-4fd7-97c2-0b16d50da6f3",
      "name": "DailyPostTrigger"
    },
    {
      "parameters": {
        "documentId": {
          "__rl": true,
          "value": "1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk",
          "mode": "list",
          "cachedResultName": "Keywords",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Sheet1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk/edit#gid=0"
        },
        "filtersUI": {
          "values": [
            {
              "lookupColumn": "scheduled_day",
              "lookupValue": "={{ $now.format('cccc') }}"
            },
            {
              "lookupColumn": "status",
              "lookupValue": "pending_approval"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -1248,
        176
      ],
      "id": "53ced7dd-2c62-4520-bc3b-4a4c26922cff",
      "name": "GetTodayPost",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "BJAdPdxFDQWm8ouf",
          "name": "Google Sheets account 2"
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
              "id": "919fcf69-c13c-4e94-8788-71a8a2a62050",
              "leftValue": "={{ $json.data.text }}",
              "rightValue": "approved",
              "operator": {
                "type": "string",
                "operation": "equals",
                "name": "filter.operator.equals"
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
        -848,
        176
      ],
      "id": "9dac9f48-1018-439d-91d0-1654865c3272",
      "name": "CheckApproval"
    },
    {
      "parameters": {
        "operation": "sendAndWait",
        "sendTo": "byangamosjerry@gmail.com",
        "subject": "Approval Required",
        "message": "=  Keyword: {{ $json.keyword }}\n  \n  Draft:\n  {{ $json.draft_content }}\n  \n  Recommended headings:\n  {{ $json.recommended_headings }}",
        "responseType": "freeText",
        "options": {}
      },
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.2,
      "position": [
        -1024,
        176
      ],
      "id": "660146d5-6e21-4b29-ad2b-28b0bc84a8b8",
      "name": "SendApprovalEmail",
      "webhookId": "16fd2bef-c0b7-4f92-9074-c02b69789d56",
      "credentials": {
        "gmailOAuth2": {
          "id": "PCc59xwnsRkP9TPX",
          "name": "Gmail account"
        }
      }
    },
    {
      "parameters": {
        "modelId": {
          "__rl": true,
          "value": "models/gemini-2.5-flash-lite",
          "mode": "list",
          "cachedResultName": "models/gemini-2.5-flash-lite"
        },
        "messages": {
          "values": [
            {
              "content": "=You are revising a blog draft based on editor feedback.\n\nOriginal draft:\n{{ $('GetTodayPost').item.json.draft_content }}\n\nEditor's notes:\n{{ $json.data.text }}\n\nRevise the draft to address the notes. Return only the revised draft text."
            }
          ]
        },
        "builtInTools": {},
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.googleGemini",
      "typeVersion": 1.2,
      "position": [
        -736,
        480
      ],
      "id": "6dc2911e-b9b7-419e-9dcf-37ffbc1effaf",
      "name": "ReviseDraft",
      "credentials": {
        "googlePalmApi": {
          "id": null,
          "name": "",
          "__aiGatewayManaged": true
        }
      }
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "5906edb2-decb-4861-970d-f7369c26a1ba",
              "name": "draft_content",
              "value": "={{ $json.content.parts[0].text }}",
              "type": "string"
            }
          ]
        },
        "includeOtherFields": true,
        "include": "selected",
        "includeFields": "keyword, recommended_headings",
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.5,
      "position": [
        -368,
        480
      ],
      "id": "a421cdf7-2502-4359-8323-42b51a603541",
      "name": "Edit Fields"
    },
    {
      "parameters": {
        "modelId": {
          "__rl": true,
          "value": "models/gemini-2.5-flash-lite",
          "mode": "list",
          "cachedResultName": "models/gemini-2.5-flash-lite"
        },
        "messages": {
          "values": [
            {
              "content": "=condense{{ $('GetTodayPost').item.json.draft_content }}  into a LinkedIn-style post (shorter, punchier, maybe with a hook and a CTA)"
            }
          ]
        },
        "builtInTools": {},
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.googleGemini",
      "typeVersion": 1.2,
      "position": [
        -640,
        176
      ],
      "id": "9996a3d2-62dd-48bd-9667-c874849ce190",
      "name": "RepurposeLinkedIn",
      "credentials": {
        "googlePalmApi": {
          "id": null,
          "name": "",
          "__aiGatewayManaged": true
        }
      }
    },
    {
      "parameters": {
        "modelId": {
          "__rl": true,
          "value": "models/gemini-2.5-flash-lite",
          "mode": "list",
          "cachedResultName": "models/gemini-2.5-flash-lite"
        },
        "messages": {
          "values": [
            {
              "content": "=condense{{ $('GetTodayPost').item.json.draft_content }}  into a newsletter section (intro line + summary + link placeholder).\nEnsure you use paragraphs and spacing for readability.\n\nOutput only the final newsletter. \nDo no included e.g \"Here's a condensed newsletter section based on your article:\\n\\n**"
            }
          ]
        },
        "builtInTools": {},
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.googleGemini",
      "typeVersion": 1.2,
      "position": [
        -656,
        304
      ],
      "id": "629e18d0-636c-40a7-994e-6631edca5092",
      "name": "RepurposeNewsletter",
      "credentials": {
        "googlePalmApi": {
          "id": null,
          "name": "",
          "__aiGatewayManaged": true
        }
      }
    },
    {
      "parameters": {
        "operation": "update",
        "documentId": {
          "__rl": true,
          "value": "1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk",
          "mode": "list",
          "cachedResultName": "Keywords",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Sheet1",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk/edit#gid=0"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "status": "=Published",
            "scheduled_day": "={{ $('GetTodayPost').item.json.scheduled_day }}"
          },
          "matchingColumns": [
            "scheduled_day"
          ],
          "schema": [
            {
              "id": "keyword",
              "displayName": "keyword",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "volume",
              "displayName": "volume",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "difficulty",
              "displayName": "difficulty",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "intent",
              "displayName": "intent",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "cpc",
              "displayName": "cpc",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "has_paa",
              "displayName": "has_paa",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "score",
              "displayName": "score",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "scheduled_day",
              "displayName": "scheduled_day",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "status",
              "displayName": "status",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": false
            },
            {
              "id": "draft_content",
              "displayName": "draft_content",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "recommended_headings",
              "displayName": "recommended_headings",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true,
              "removed": true
            },
            {
              "id": "row_number",
              "displayName": "row_number",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "number",
              "canBeUsedToMatch": true,
              "readOnly": true,
              "removed": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -320,
        16
      ],
      "id": "b8366dc3-b858-419e-a051-2501eb63cd0d",
      "name": "UpdateStatus",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "BJAdPdxFDQWm8ouf",
          "name": "Google Sheets account 2"
        }
      }
    },
    {
      "parameters": {
        "sendTo": "byangamosjerry@gmail.com",
        "subject": "Unlock Your Business Potential with AI Automation",
        "emailType": "text",
        "message": "={{ $json.content.parts[0].text }}",
        "options": {}
      },
      "type": "n8n-nodes-base.gmail",
      "typeVersion": 2.2,
      "position": [
        -288,
        304
      ],
      "id": "e61c943d-21b6-4b7e-98d8-41bead541e6a",
      "name": "PublishNewsletter",
      "webhookId": "e6e32e97-1ad1-4b3f-b938-8b93e27cf33c",
      "credentials": {
        "gmailOAuth2": {
          "id": "PCc59xwnsRkP9TPX",
          "name": "Gmail account"
        }
      }
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.noOp",
      "typeVersion": 1,
      "position": [
        544,
        -512
      ],
      "id": "72674adc-a970-41e9-8156-81acf1a1ecfc",
      "name": "No Operation, do nothing"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.noOp",
      "typeVersion": 1,
      "position": [
        -96,
        16
      ],
      "id": "c2590fd7-15db-49ea-8898-614332122aec",
      "name": "No Operation, do nothing1"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.noOp",
      "typeVersion": 1,
      "position": [
        -64,
        304
      ],
      "id": "971c1016-3195-4015-953d-725232f7f59e",
      "name": "No Operation, do nothing2"
    },
    {
      "parameters": {},
      "type": "n8n-nodes-base.noOp",
      "typeVersion": 1,
      "position": [
        -768,
        0
      ],
      "id": "89c6df94-1156-4180-8321-f742582f9c45",
      "name": "No Operation, do nothing3"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://webhook.site/dcfb0e98-3221-4511-97f6-75a9e3e54c68",
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "={{ {\n  \"keyword\": $json.keyword,\n  \"draft_content\": $json.draft_content\n} }}",
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.5,
      "position": [
        -528,
        16
      ],
      "id": "7fac3d1c-8836-4421-b66b-a3daa635c69d",
      "name": "PublishWebsite"
    },
    {
      "parameters": {
        "jsCode": "const groups = $input.all();\nconst domainSet = new Set();\n\ngroups.forEach(item => {\n  (item.json.pages || []).forEach(p => {\n    if (p.url) {\n      try {\n        const domain = new URL(p.url).hostname.replace(/^www\\./, '');\n        domainSet.add(domain);\n      } catch (e) {}\n    }\n  });\n});\n\nreturn Array.from(domainSet).map(domain => ({ json: { competitor_domain: domain } }));"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -1440,
        -656
      ],
      "id": "eb65bfc1-5b27-4e5a-8cdb-920f3d3e61b2",
      "name": "ExtractCompetitorDomains"
    },
    {
      "parameters": {
        "method": "POST",
        "url": "https://api.dataforseo.com/v3/backlinks/referring_domains/live",
        "authentication": "genericCredentialType",
        "genericAuthType": "httpBasicAuth",
        "sendBody": true,
        "specifyBody": "json",
        "jsonBody": "=[\n  {\n    \"target\": \"{{ $json.competitor_domain }}\",\n    \"limit\": 5,\n    \"order_by\": [\"rank,desc\"]\n  }\n]",
        "options": {}
      },
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.5,
      "position": [
        -1216,
        -656
      ],
      "id": "268164c4-9508-415e-a9ca-3d17e104f8e6",
      "name": "GetReferringDomains",
      "credentials": {
        "httpBasicAuth": {
          "id": "A0HdyeiCt5yD5Rfm",
          "name": "DataForSEO API"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "const responses = $input.all();\nconst domains = $('ExtractCompetitorDomains').all();\n\nconst rows = [];\nresponses.forEach((resp, i) => {\n  const competitor = domains[i]?.json?.competitor_domain || 'unknown';\n  const items = resp.json.tasks?.[0]?.result?.[0]?.items || [];\n  items.forEach(d => {\n    rows.push({\n      json: {\n        competitor_domain: competitor,\n        referring_domain: d.domain,\n        domain_rank: d.rank,\n        backlinks: d.backlinks,\n        why_fit: `Links to ${competitor}, a site ranking for your target keywords`,\n        outreach_angle: 'Guest post pitch or resource link request',\n        date_added: new Date().toISOString().slice(0, 10)\n      }\n    });\n  });\n});\n\nreturn rows;"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        -992,
        -656
      ],
      "id": "3ca9a36d-9887-4458-a566-cda59dbd0ce7",
      "name": "ParseBacklinks"
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "1TFRMF6zj-1w7J-nqYxUTZ7cg4dHQzSDF244Hs9lXxjk",
          "mode": "list",
          "cachedResultName": "Keywords"
        },
        "sheetName": {
          "__rl": true,
          "value": "BacklinkTargets",
          "mode": "list",
          "cachedResultName": "BacklinkTargets"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "competitor_domain": "={{ $json.competitor_domain }}",
            "referring_domain": "={{ $json.referring_domain }}",
            "domain_rank": "={{ $json.domain_rank }}",
            "backlinks": "={{ $json.backlinks }}",
            "why_fit": "={{ $json.why_fit }}",
            "outreach_angle": "={{ $json.outreach_angle }}",
            "date_added": "={{ $json.date_added }}"
          },
          "matchingColumns": [],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        -768,
        -656
      ],
      "id": "54e22466-b2aa-4ed1-8c27-e9ee0bc3b485",
      "name": "SaveBacklinkTargets",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "BJAdPdxFDQWm8ouf",
          "name": "Google Sheets account 2"
        }
      }
    }
  ],
  "connections": {
    "WeeklyTrigger": {
      "main": [
        [
          {
            "node": "GetKeywords",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "GetKeywords": {
      "main": [
        [
          {
            "node": "SplitKeywords",
            "type": "main",
            "index": 0
          },
          {
            "node": "ExtractCompetitorDomains",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "FilterKeywords": {
      "main": [
        [
          {
            "node": "CombineKeywords",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "CombineKeywords": {
      "main": [
        [
          {
            "node": "JudgeRelevance",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "JudgeRelevance": {
      "main": [
        [
          {
            "node": "ScoreKeywords",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "GetExistingKeywords": {
      "main": [
        [
          {
            "node": "CheckDuplicateKeywords",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "CheckDuplicateKeywords": {
      "main": [
        [
          {
            "node": "TopUpFromRelated",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "SaveKeywords": {
      "main": [
        [
          {
            "node": "No Operation, do nothing",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "SplitKeywords": {
      "main": [
        [
          {
            "node": "FilterKeywords",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ScoreKeywords": {
      "main": [
        [
          {
            "node": "GetExistingKeywords",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "RankAndCap": {
      "main": [
        [
          {
            "node": "SaveKeywords",
            "type": "main",
            "index": 0
          },
          {
            "node": "GetSERP",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "TopUpFromRelated": {
      "main": [
        [
          {
            "node": "RankAndCap",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "GetSERP": {
      "main": [
        [
          {
            "node": "ParseSERP",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ParseSERP": {
      "main": [
        [
          {
            "node": "SplitByResult",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "SplitByResult": {
      "main": [
        [
          {
            "node": "TrimToTop3",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "TrimToTop3": {
      "main": [
        [
          {
            "node": "FetchCompetitorPages",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "FetchCompetitorPages": {
      "main": [
        [
          {
            "node": "ExtractArticleText",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "HandleFetchErrors",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ExtractArticleText": {
      "main": [
        [
          {
            "node": "RestoreKeywordFields",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "HandleFetchErrors": {
      "main": [
        [
          {
            "node": "Merge",
            "type": "main",
            "index": 1
          }
        ]
      ]
    },
    "TrimBodyText": {
      "main": [
        [
          {
            "node": "GroupByKeyword",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "GroupByKeyword": {
      "main": [
        [
          {
            "node": "CompetitorBrief",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "RestoreKeywordFields": {
      "main": [
        [
          {
            "node": "Merge",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Merge": {
      "main": [
        [
          {
            "node": "TrimBodyText",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "CompetitorBrief": {
      "main": [
        [
          {
            "node": "SaveBrief",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "SaveBrief": {
      "main": [
        [
          {
            "node": "DraftBlogPost",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "DraftBlogPost": {
      "main": [
        [
          {
            "node": "MergeKeywordRow",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "MergeKeywordRow": {
      "main": [
        [
          {
            "node": "UpdateDraftStatus",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "UpdateDraftStatus": {
      "main": [
        [
          {
            "node": "No Operation, do nothing3",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "DailyPostTrigger": {
      "main": [
        [
          {
            "node": "GetTodayPost",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "GetTodayPost": {
      "main": [
        [
          {
            "node": "SendApprovalEmail",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "CheckApproval": {
      "main": [
        [
          {
            "node": "PublishWebsite",
            "type": "main",
            "index": 0
          },
          {
            "node": "RepurposeLinkedIn",
            "type": "main",
            "index": 0
          },
          {
            "node": "RepurposeNewsletter",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "ReviseDraft",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "SendApprovalEmail": {
      "main": [
        [
          {
            "node": "CheckApproval",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ReviseDraft": {
      "main": [
        [
          {
            "node": "Edit Fields",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Edit Fields": {
      "main": [
        [
          {
            "node": "SendApprovalEmail",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "RepurposeLinkedIn": {
      "main": [
        []
      ]
    },
    "RepurposeNewsletter": {
      "main": [
        [
          {
            "node": "PublishNewsletter",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "UpdateStatus": {
      "main": [
        [
          {
            "node": "No Operation, do nothing1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "PublishNewsletter": {
      "main": [
        [
          {
            "node": "No Operation, do nothing2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "PublishWebsite": {
      "main": [
        [
          {
            "node": "UpdateStatus",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ExtractCompetitorDomains": {
      "main": [
        [
          {
            "node": "GetReferringDomains",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "GetReferringDomains": {
      "main": [
        [
          {
            "node": "ParseBacklinks",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ParseBacklinks": {
      "main": [
        [
          {
            "node": "SaveBacklinkTargets",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "pinData": {},
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "099b152af6c57e29621952263b5da871c3feb2e97d0f34a3a05ff78d0e11628a"
  }
}
