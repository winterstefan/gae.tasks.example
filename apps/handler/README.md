# Example project for 10-task-limit

This source code was taken from the [official php-docs-sample repository](https://github.com/GoogleCloudPlatform/php-docs-samples/tree/master/appengine/php72/tasks/apps/handler).

## Basics

This is a dummy project, configured with a small queue and task handler. Each task will take
about 10 minutes to process (simple `sleep()`), the queue is configured to run 1.000 tasks at most.

The GAE is configured to have up to 3 instances running. 

## How to reproduce

1. Check out this source code
2. Optional: Create new Google Cloud project
3. Use `gcloud` CLI to deploy both app and queue: `gcloud app deploy app.yaml queue.yaml`
4. Visit [queue detail view](https://console.cloud.google.com/cloudtasks/queue/my-queue) and **pause** the queue
5. Generate 100 sample tasks, for example via CLI (or possibly via `snippets/src/create_task.php`)
    ```
   gcloud tasks create-app-engine-task  --queue=my-queue --relative-uri="/task_handler" --method="POST" --body-content="hello1" --project=[YOUR-PROJECT-ID]
   ```
6. **Unpause** queue, wait a couple of seconds and view number of running tasks.


## Expected behaviour

All 100 tasks from the queue are marked as in progress.

## My current behaviour

Just 30 tasks are being processed at a time. If I modify the `basic_scaling.max_instances`,
I'll get a hard limit of 
`10 * [RUNNING INSTANCE COUNT]` (e.g. 50 running for 5 instances, 70 running for 7 instances).