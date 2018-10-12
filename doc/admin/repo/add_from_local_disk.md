## Add repositories already cloned to disk

You can also add repositories to Sourcegraph that are already cloned to disk on the host machine. This is useful for repositories requiring non-standard authentication to clone, or very large repositories on which cloning exceeds the resources available to the Docker container.

The steps documented here are intended for Sourcegraph instances running on a single node. The general process also applies to Sourcegraph Data Center (running on Kubernetes), but you need to perform these steps on the underlying node hosting the `gitserver` pod, or on the persistent volume used by the `gitserver` deployment.

If you're using the default `--volume $HOME/.sourcegraph/data:/var/opt/sourcegraph` argument to run the `sourcegraph/server` Docker image, and the repository you want to add is named `github.com/my/repo`, then follow these steps:

1.  If Sourcegraph is running, ensure the repository is disabled so it doesn't attempt to clone it.

1.  On the host machine, ensure that a bare Git clone of the repository exists at `$HOME/.sourcegraph/data/repos/github.com/my/repo/.git`.

    To create a new clone given its clone URL:

    ```
    git clone --mirror YOUR-REPOSITORY-CLONE-URL $HOME/.sourcegraph/data/repos/github.com/my/repo/.git
    ```

    Or, as an optimization, you can reuse an existing local clone to avoid needing to fetch all the repository data again:

    ```
    git clone --mirror --reference PATH-TO-YOUR-EXISTING-LOCAL-CLONE --dissociate YOUR-REPOSITORY-CLONE-URL $HOME/.sourcegraph/data/repos/github.com/my/repo/.git
    ```

1.  Ensure your site configuration contains entries for the code host of the added repository and then enable the repository in the admin UI.

    If this repository exists on a code host that Sourcegraph directly integrates with, then use that code host's configuration (as described in the other section on this page). After updating the site configuration, if you used the correct repository path, Sourcegraph will detect and reuse the existing clone. (For example, if you're working with a repository on GitHub.com, ensure that the repository path name you used is of the form `github.com/my/repo`.)

    Otherwise, add an entry for this repository using [`repos.list`](#sync-repositories-from-any-code-host) and use a `url` (clone URL) of `file:///var/opt/sourcegraph/repos/github.com/my/repo`.