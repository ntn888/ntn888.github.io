+++
title = "Perpertually AI auto blogging from RSS news feed: My experience with NO-CODE tools"
date = 2024-10-05 04:27:00
draft = false

[taxonomies]
categories = ["misc"]
tags = ["no-code", "workflow", "AI blogging howto"]

[extra]
lang = "en"
toc = true
comment = false
copy = true
math = false
mermaid = false
outdate_alert = false
outdate_alert_days = 120
display_tags = true
truncate_summary = false
+++

Enter [n8n](https://n8n.io/), an up-and-comming alternative to Make.com; that helps create a drag-and-drop workflow for building web pipilines without coding. The distinguishing of n8n is that it is self-hostable without any license and is released as opensource.

With the ever shrinking AI costs, and with AI agents being all the rage on Youtube tutorials, I felt an itch to try them out... naturally.

Most of these tutorials featured web workflows with Make.com. I tilted towards the selfhosted n8n... again, naturally. These workflows can be linked to OpenAI API to build agents in the workflow. And the utility of the combination is mindblowing!! Especially ease of use, for the layman!

And so I set out to implement fully automated blogging from news source - without any human intervention. I chose to use the GPT 4o-mini model as it's the most cost effective. One article costs under 5c to generate.

In creating the workflow I had one initial major hurdle - enabling the AI agent to access the news source web page. I got onto the n8n community forum, and boy! I recieved a prompt response from the staff with a specific implementation example for my usecase!

The final workflow looks like this:

![n8n Workflow screen](/img/n8n-autoblogging-workflow.png)

```
{
  "meta": {
    "instanceId": "3ff62b04b44d3432aa968154a94112c96c611f8f64771b1525f31b170fd94df5"
  },
  "nodes": [
    {
      "parameters": {
        "model": "gpt-4o-mini",
        "options": {
          "maxTokens": 1000
        }
      },
      "id": "043f35ee-3851-49ae-94e5-15f4c03d5672",
      "name": "OpenAI Chat Model",
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenAi",
      "typeVersion": 1,
      "position": [
        1100,
        660
      ],
      "credentials": {
        "openAiApi": {
          "id": "HwtpRSRN6gaCQp8N",
          "name": "OpenAi account"
        }
      }
    },
    {
      "parameters": {},
      "id": "56356c70-c111-4e86-b612-00ed5f6618c7",
      "name": "Limit",
      "type": "n8n-nodes-base.limit",
      "typeVersion": 1,
      "position": [
        660,
        260
      ]
    },
    {
      "parameters": {
        "model": "gpt-4o-mini",
        "options": {
          "maxTokens": 1000
        }
      },
      "id": "b1f8a92e-ceb5-4742-af7d-ebc4f23e01c7",
      "name": "OpenAI Chat Model1",
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenAi",
      "typeVersion": 1,
      "position": [
        1440,
        680
      ],
      "credentials": {
        "openAiApi": {
          "id": "HwtpRSRN6gaCQp8N",
          "name": "OpenAi account"
        }
      }
    },
    {
      "parameters": {
        "url": "={{ $json.link }}",
        "sendQuery": true,
        "queryParameters": {
          "parameters": [
            {
              "name": "headers",
              "value": "'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.0.0 Safari/537.36'"
            },
            {
              "name": "cookies",
              "value": "'CONSENT': 'YES+cb.20220419-08-p0.cs+FX+111'"
            }
          ]
        },
        "options": {
          "redirect": {
            "redirect": {}
          }
        }
      },
      "id": "c6186aa5-b187-4b29-8188-2106fb7c610e",
      "name": "HTTP Request",
      "type": "n8n-nodes-base.httpRequest",
      "typeVersion": 4.2,
      "position": [
        880,
        260
      ]
    },
    {
      "parameters": {
        "operation": "extractHtmlContent",
        "extractionValues": {
          "values": [
            {
              "key": "body",
              "cssSelector": "#article-body"
            }
          ]
        },
        "options": {
          "cleanUpText": true
        }
      },
      "id": "77f14452-56ff-495d-9de2-c1da18c0fd36",
      "name": "HTML",
      "type": "n8n-nodes-base.html",
      "typeVersion": 1.2,
      "position": [
        1100,
        260
      ]
    },
    {
      "parameters": {
        "operation": "toText",
        "sourceProperty": "result",
        "options": {
          "fileName": "={{ $('Generate URL SLUG').item.json.text }}.md"
        }
      },
      "id": "d4266fdc-6a6e-4786-adfc-a3971a8a6ecb",
      "name": "Convert to File",
      "type": "n8n-nodes-base.convertToFile",
      "typeVersion": 1.1,
      "position": [
        2720,
        460
      ]
    },
    {
      "parameters": {
        "mode": "raw",
        "jsonOutput": "={\n\n\"meta\": \"+++\\ntitle = \\\"{{ $('Gen Title').item.json.text }}\\\"\\ndate = {{ $now.format(\"yyyy-MM-dd HH:mm:ss\") }}\\ndraft = false\\n\\n[taxonomies]\\ncategories = [\\\"Tech News\\\"]\\ntags = [\\\"Tech News\\\"]\\n\\n[extra]\\nlang = \\\"en\\\"\\ntoc = true\\ncomment = false\\ncopy = true\\nmath = false\\nmermaid = false\\noutdate_alert = false\\noutdate_alert_days = 120\\ndisplay_tags = true\\ntruncate_summary = false\\n+++\\n\\n\"\n}",
        "options": {}
      },
      "id": "d62388c7-065e-4e20-8c1d-ca3d46cb0f48",
      "name": "add_meta",
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        2240,
        460
      ]
    },
    {
      "parameters": {
        "jsCode": "let z = $input.first().json.meta;\nz = z.concat($('Gen Article').first().json.text);\n\n\n//let z = Object.assign($input.first().json.meta, $input.first().json.text);\n//let z = $input.first().json.meta\n\n\nreturn {\n  json: {\n    result: z // Return the result text\n  }\n}; "
      },
      "id": "68ccc345-aa70-4565-8709-1133e96ad688",
      "name": "Code",
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        2460,
        460
      ]
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "=Generate a post on this article: {{ $('HTML').item.json.body }}\n\nHere's the title of the article: {{ $json.text }}\n\nYou're a blog writer and you are turning the article given above into a unique SEO-optimised blog post. DO NOT include the title as it's already written so please start with the body to the article.\n\nFormatting: Use H1, H2 and H3 headers. You choose what to bold and bullet point.\n\nParameters: This is a blog post and it should be approx. 500 words long."
      },
      "id": "ad801655-230e-489d-91dc-6df5f10e5f9c",
      "name": "Gen Article",
      "type": "@n8n/n8n-nodes-langchain.chainLlm",
      "typeVersion": 1.4,
      "position": [
        1440,
        460
      ]
    },
    {
      "parameters": {
        "model": "gpt-4o-mini",
        "options": {
          "maxTokens": 1000
        }
      },
      "id": "24634b0d-c6e3-4113-9bba-7a386b6b7620",
      "name": "OpenAI Chat Model2",
      "type": "@n8n/n8n-nodes-langchain.lmChatOpenAi",
      "typeVersion": 1,
      "position": [
        1860,
        680
      ],
      "credentials": {
        "openAiApi": {
          "id": "HwtpRSRN6gaCQp8N",
          "name": "OpenAi account"
        }
      }
    },
    {
      "parameters": {
        "resource": "file",
        "owner": {
          "__rl": true,
          "value": "ntn888",
          "mode": "name"
        },
        "repository": {
          "__rl": true,
          "value": "ecosmartabode_online",
          "mode": "list",
          "cachedResultName": "ecosmartabode_online",
          "cachedResultUrl": "https://github.com/ntn888/ecosmartabode_online"
        },
        "filePath": "=content/blog/{{ $('Generate URL SLUG').item.json.text }}.md",
        "binaryData": true,
        "commitMessage": "=create post: {{ $('Generate URL SLUG').item.json.text }}",
        "additionalParameters": {
          "branch": {
            "branch": "main"
          }
        }
      },
      "id": "c4aed289-807d-4388-8a74-55d5865c6802",
      "name": "GitHub",
      "type": "n8n-nodes-base.github",
      "typeVersion": 1,
      "position": [
        3000,
        460
      ],
      "credentials": {
        "githubApi": {
          "id": "JfzHKbfULAozXWJ2",
          "name": "GitHub account"
        }
      }
    },
    {
      "parameters": {
        "pollTimes": {
          "item": [
            {
              "mode": "everyHour"
            }
          ]
        },
        "feedUrl": "https://www.techradar.com/rss"
      },
      "id": "9ea6652a-c6c1-4ff2-b0f2-51b6a7097471",
      "name": "RSS Feed Trigger",
      "type": "n8n-nodes-base.rssFeedReadTrigger",
      "typeVersion": 1,
      "position": [
        320,
        260
      ]
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "=You're a blog writer and your job is to turn the following news article into a unique SEO optimised blog post.\n\nHere's the title of the article: {{ $('Limit').item.json.title }}\nHere's the article, please read it here: {{ $json.body }}\n\nBased on the information given above, please generate an SEO Optimised Article Title.\n\nParameters:\n\nMax 10 words & 1 sentence flow\n\nDO NOT put quotes around the title"
      },
      "id": "1ee9c123-9a81-445a-b092-03142589e219",
      "name": "Gen Title",
      "type": "@n8n/n8n-nodes-langchain.chainLlm",
      "typeVersion": 1.4,
      "position": [
        1080,
        460
      ],
      "executeOnce": false
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "=Generate a short URL SLUG for the blog post titled:  {{ $('Gen Title').item.json.text }}\n\nKeep it short and DO NOT include the domain name. Provide only the relative path. DO NOT include any slashes."
      },
      "id": "18da5708-1d1c-4af7-bc62-ff5c284c3dfa",
      "name": "Generate URL SLUG",
      "type": "@n8n/n8n-nodes-langchain.chainLlm",
      "typeVersion": 1.4,
      "position": [
        1840,
        460
      ]
    }
  ],
  "connections": {
    "OpenAI Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "Gen Title",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Limit": {
      "main": [
        [
          {
            "node": "HTTP Request",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "OpenAI Chat Model1": {
      "ai_languageModel": [
        [
          {
            "node": "Gen Article",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "HTTP Request": {
      "main": [
        [
          {
            "node": "HTML",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "HTML": {
      "main": [
        [
          {
            "node": "Gen Title",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Convert to File": {
      "main": [
        [
          {
            "node": "GitHub",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "add_meta": {
      "main": [
        [
          {
            "node": "Code",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Code": {
      "main": [
        [
          {
            "node": "Convert to File",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Gen Article": {
      "main": [
        [
          {
            "node": "Generate URL SLUG",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "OpenAI Chat Model2": {
      "ai_languageModel": [
        [
          {
            "node": "Generate URL SLUG",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "RSS Feed Trigger": {
      "main": [
        [
          {
            "node": "Limit",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Gen Title": {
      "main": [
        [
          {
            "node": "Gen Article",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Generate URL SLUG": {
      "main": [
        [
          {
            "node": "add_meta",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "pinData": {}
}
```

It's pretty much self-explainatory. The first node *RSS Feed Trigger* runs periodically and pulls in the latest news item. The end node is used to commit our new post content to the Github repo (where the code for my SSG website resides). Be sure to modify this value to target your own repo. You can simply copy and paste this workflow code into your instance of n8n!

Finally you may see the results produced on my test blog here: [https://ecosmartabode.online/blog/](https://ecosmartabode.online/blog/)


>Note: I gathered the AI prompts from this [Youtube tutorial](https://www.youtube.com/watch?v=u5TyXyf1jRk).
