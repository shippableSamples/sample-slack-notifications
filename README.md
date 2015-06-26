Sample of Slack notifications integration
=========================================

This sample demonstrates how to add Slack build notifications to a Rails+Postgres application.

The integration with Slack is achieved using so called 'incoming webhooks'.
Basically, these are Slack RESTful API endpoints that allow you to send message to the channel.
Please refer to the documentation linked below for details how to add an incoming webhook to your
organization and channel.

All the heavy-lifting of sending the message to Slack is performed by `slack_notifier.py` script
that is included in this repository. You can freely copy it to your project to setup the
integration.

The script requires the following arguments:

* `--project` -- the human-readable name of your project
* `--token` -- your Slack API token that is assigned during webhook creation

By default, the build is marked as failure. To mark it as success, add `-s` option. You will most
likely prefer to save the values for parameters above as environment variables, with Slack token
being an encrypted one:

    - PROJECT=sample-rubyonrails-postgres-heroku
    - secure: MRuHkLbL9HPkJPU5lzkKM1+NOq1S5RrhxEyhJkk60xxYiF7DMzydiBN8oFBjWrSmyGeGRuEC22a0I5ItobdWVszfcJCaXHwtfKzfGOUdKuyCnDgvojXhv/jrBvULyLK6zsLw3b8NMxdnwNsHqSPm19qW/EIGEl9Zv/637Igos69z9aT7+xrEG013+6HtKYb8RHm+iPSNsFoBi/RSAHYuM1eLTZWG2WAkjgzZaYmrHCgNwVmk+HOGR+TOWN7Iu5lrjyvC1XDCQrOvo1hZI30cd9OqJ5aadFm3exQpNhI4I7AgOnCbK3NoWNc/GAnqKXCvsaIQ80Jd/uLIOVyMjD6Xmg==

Then, you can add `after_failure` and `after_success` steps as follows:

    after_failure:
      - python slack_notifier.py --project $PROJECT --token $SLACK_TOKEN

    after_success:
      - python slack_notifier.py --project $PROJECT --token $SLACK_TOKEN -s

For more detailed documentation, please see:

* Shippable's continuous deployment section: http://docs.shippable.com/en/latest/config.html#continuous-deployment
* Slack integration documentation: https://my.slack.com/services/new/incoming-webhook

This sample is built for Shippable, a docker based continuous integration and deployment platform.
