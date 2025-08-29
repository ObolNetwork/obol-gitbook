# Update a DV

It is highly recommended to upgrade your DV stack from time to time. This ensures that your node is secure, performant, up-to-date and you don't miss important hard forks.

To do this, follow these steps:

{% tabs %}
{% tab title="charon-distributed-validator-node" %}
```sh
cd charon-distributed-validator-node
```

#### Pull latest changes to the repo[​](https://docs.obol.org/next/run/running/update#pull-latest-changes-to-the-repo) <a href="#pull-latest-changes-to-the-repo" id="pull-latest-changes-to-the-repo"></a>

```sh
git pull
```

#### Create (or recreate) your DV stack[​](https://docs.obol.org/next/run/running/update#create-or-recreate-your-dv-stack) <a href="#create-or-recreate-your-dv-stack" id="create-or-recreate-your-dv-stack"></a>

```sh
docker compose up -d --build
```

{% hint style="danger" %}
If you run more than one node in a DV Cluster, please take caution upgrading them simultaneously. Particularly if you are updating or changing the validator client used or recreating disks. It is recommended to update nodes on a sequential basis to minimise liveness and safety risks.
{% endhint %}

#### Conflicts[​](https://docs.obol.org/next/run/running/update#conflicts) <a href="#conflicts" id="conflicts"></a>

You may get a `git conflict` error similar to this:

```sh
error: Your local changes to the following files would be overwritten by merge:
prometheus/prometheus.yml

Please commit your changes or stash them before you merge.
```

This is probably because you have made some changes to some of the files, for example to the `prometheus/prometheus.yml` file.

To resolve this error, you can either:

*   Stash and reapply changes if you want to keep your custom changes:

    ```sh
    git stash                       # Stash your local changes
    git pull                        # Pull the latest changes
    git stash apply                 # Reapply your changes from the stash
    docker-compose up -d --build    # Recreate your DV stack
    ```

    After reapplying your changes, manually resolve any conflicts that may arise between your changes and the pulled changes using a text editor or Git's conflict resolution tools.
*   Override changes and recreate configuration if you don't need to preserve your local changes and want to discard them entirely:

    ```sh
    git reset --hard                # Discard all local changes and override with the pulled changes
    git pull                        # Pull the latest changes
    docker-compose up -d --build    # Recreate your DV stack
    ```

    After overriding the changes, you will need to recreate your DV stack using the updated files. By following one of these approaches, you should be able to handle Git conflicts when pulling the latest changes to your repository, either preserving your changes or overriding them as per your requirements.
{% endtab %}

{% tab title="charon-distributed-validator-cluster" %}
```sh
cd charon-distributed-validator-cluster
```

#### Pull latest changes to the repo[​](https://docs.obol.org/next/run/running/update#pull-latest-changes-to-the-repo) <a href="#pull-latest-changes-to-the-repo" id="pull-latest-changes-to-the-repo"></a>

```sh
git pull
```

#### Create (or recreate) your DV stack[​](https://docs.obol.org/next/run/running/update#create-or-recreate-your-dv-stack) <a href="#create-or-recreate-your-dv-stack" id="create-or-recreate-your-dv-stack"></a>

```sh
docker compose up -d --build
```

{% hint style="danger" %}
If you run more than one node in a DV Cluster, please take caution upgrading them simultaneously. Particularly if you are updating or changing the validator client used or recreating disks. It is recommended to update nodes on a sequential basis to minimise liveness and safety risks.
{% endhint %}

#### Conflicts[​](https://docs.obol.org/next/run/running/update#conflicts) <a href="#conflicts" id="conflicts"></a>

You may get a `git conflict` error similar to this:

```sh
error: Your local changes to the following files would be overwritten by merge:
prometheus/prometheus.yml

Please commit your changes or stash them before you merge.
```

This is probably because you have made some changes to some of the files, for example to the `prometheus/prometheus.yml` file.

To resolve this error, you can either:

*   Stash and reapply changes if you want to keep your custom changes:

    ```sh
    git stash                       # Stash your local changes
    git pull                        # Pull the latest changes
    git stash apply                 # Reapply your changes from the stash
    docker-compose up -d --build    # Recreate your DV stack
    ```

    After reapplying your changes, manually resolve any conflicts that may arise between your changes and the pulled changes using a text editor or Git's conflict resolution tools.
*   Override changes and recreate configuration if you don't need to preserve your local changes and want to discard them entirely:

    ```sh
    git reset --hard                # Discard all local changes and override with the pulled changes
    git pull                        # Pull the latest changes
    docker-compose up -d --build    # Recreate your DV stack
    ```

    After overriding the changes, you will need to recreate your DV stack using the updated files. By following one of these approaches, you should be able to handle Git conflicts when pulling the latest changes to your repository, either preserving your changes or overriding them as per your requirements.
{% endtab %}
{% endtabs %}

