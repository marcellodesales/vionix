# 📣 Slack Notification with Mentions Support

* Submit slack messages using full-automated and generalized messages using any of the APIs
  * Text
  * Attachments
* Integration with any Enterprise Slack, including Viasat's accounts

# :wrench: Settings

* Inputs

* `channelId`: The id of the channel, captured at the channel's settings
* `messageId`: When provided, it updates an existing message
* `slackBotToken`: The slack channel app token from your Slack Bot Oauth 
* `payload`: The payload provided in the API

Examples of this pattern include the following use cases:

# Use Cases

## Single Message to Channel

* Using the attachments API, you can build rich messages

```yaml
      - id: recorder_message
        name: Add Recorder PR review message
        uses: seceng-devsecops-platform/devsecops-platform-github-workflows/actions/messaging/slack-notification@main
        with:
          channelId: ${{ inputs.channelId }}
          slackBotToken: ${{ secrets.SLACK_CHANNEL_AUTOMATION_TOKEN }}
          payload: >-
            {
                "text": "🔓 Review the initial PR ${{ inputs.initialPrNumber }} with your Vionix GHAS App config",
                "thread_ts": "${{ steps.thread_setup.outputs.threadId }}",
                "attachments": [
                    {
                        "color": "warning",
                        "fields": [
                            {
                              "title": ":prmerge: Record PR Merged!",
                              "value": "<${{ github.server_url }}/${{ inputs.gitOrg }}/.github/pull/${{ inputs.recorderPrNumber }}|PR #${{ inputs.recorderPrNumber }}>",
                              "short": true
                            },
                            {
                              "title": "🔧 GHAS Config",
                              "value": "<${{ github.server_url }}/${{ inputs.gitOrg }}/.github/blob/main/vionix-platform/products/ghas-${{ inputs.githubAppInstallationEnv }}.yaml|ghas-${{ inputs.githubAppInstallationEnv }}.yaml>",
                              "short": true
                            },
                            {
                              "title": ":workingonit: Status",
                              "value": "${{ steps.owners_profile.outputs.email }}, waiting on initial the GHAS enablement by GIAPPS",
                              "short": false
                            }
                        ],
                        "footer": "🤖 Automated by Vionix DevSecOps Platform :viasat-signal-animated-transparent:"
                    }
                ]
            }
```

## Message Threads

* Provide the `threadId` of the message within the payload so that you can write messages inside a thread

## Update of Messages

* Provide the `messageId` parameter

