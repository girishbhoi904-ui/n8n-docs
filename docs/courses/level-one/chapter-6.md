---{
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
        "resource": "video",
        "filters": {}
      },
      "id": "trigger-node",
      "name": "YouTube Trigger",
      "type": "n8n-nodes-base.youTubeTrigger",
      "typeVersion": 1,
      "position": [100, 300]
    },
    {
      "parameters": {
        "model": "gpt-4o",
        "messages": {
          "messageValues": [
            {
              "role": "system",
              "content": "You are a YouTube SEO expert. Write a professional, engaging description with hashtags."
            },
            {
              "content": "=Write a catchy YouTube description for a video titled: {{$json.snippet.title}}. Also include relevant hashtags."
            }
          ]
        }
      },
      "id": "openai-node",
      "name": "ChatGPT SEO",
      "type": "n8n-nodes-base.openAi",
      "typeVersion": 1.2,
      "position": [320, 300],
      "credentials": {
        "openAiApi": {
          "id": "YOUR_OPENAI_ID",
          "name": "OpenAI Account"
        }
      }
    },
    {
      "parameters": {
        "resource": "video",
        "operation": "update",
        "videoId": "={{$node[\"YouTube Trigger\"].json[\"id\"][\"videoId\"]}}",
        "snippet": {
          "title": "={{$node[\"YouTube Trigger\"].json[\"snippet\"][\"title\"]}}",
          "description": "={{$json.choices[0].message.content}}"
        }
      },
      "id": "update-node",
      "name": "YouTube Update",
      "type": "n8n-nodes-base.youTube",
      "typeVersion": 1,
      "position": [540, 300],
      "credentials": {
        "youTubeOAuth2Api": {
          "id": "YOUR_YOUTUBE_ID",
          "name": "YouTube Account"
        }
      }
    }
  ],
  "connections": {
    "YouTube Trigger": {
      "main": [
        [
          {
            "node": "ChatGPT SEO",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "ChatGPT SEO": {
      "main": [
        [
          {
            "node": "YouTube Update",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  }
}

contentType: tutorial
---

<!-- vale from-microsoft.We = NO -->
<!-- vale from-microsoft.FirstPerson = NO -->
# Exporting and importing workflows

In this chapter, you will learn how to export and import workflows.

## Exporting and importing workflows

You can save n8n workflows locally as JSON files. This is useful if you want to share your workflow with someone else or import a workflow from someone else.

--8<-- "_snippets/workflows/sharing-credentials.md"

<figure><img src="/_images/courses/level-one/chapter-six/l1-c6-import-export-menu.png" alt="Import/Export menu" style="width:100%"><figcaption align = "center"><i>Import & Export workflows menu</i></figcaption></figure>

You can export and import workflows in three ways:

* From the **Editor UI** menu:
    * Export: From the top navigation bar, select the three dots in the upper right, then select **Download**. This will download your current workflow as a JSON file on your computer.
    * Import: From the top navigation bar, select the three dots in the upper right, then select **Import from URL** (to import a published workflow) or **Import from File** (to import a workflow as a JSON file).
* From the **Editor UI** canvas:
	* Export: Select all the nodes on the canvas and use ++ctrl+c++ to copy the workflow JSON. You can paste this into a file or share it directly with other people.
	* Import: You can paste a copied workflow JSON directly into the canvas with ++ctrl+v++.
* From the command line:
    * Export: See the [full list of commands ](/hosting/cli-commands.md) for exporting workflows or credentials.
    * Import: See the [full list of commands ](/hosting/cli-commands.md#import-workflows-and-credentials) for importing workflows or credentials.
