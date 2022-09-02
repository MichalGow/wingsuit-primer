# Wingsuit Primer

This repository is a fork of [Wingsuit Kickstarter](https://github.com/wingsuit-designsystem/wingsuit-kickstarter),
a demo project to show how to use [Storybook](https://storybook.js.org/)
with [Drupal](https://www.drupal.org/) via [Wingsuit](https://wingsuit-designsystem.github.io/).

This combination address one of the biggest criticisms against Drupal, that the frontend development
has a too steep learning curve, and that it requires only Drupal professionals.

With [Storybook](https://storybook.js.org/), it does not. All the designer/frontend developer needs to
know are frontend technologies like CSS, JS and Twig. See below for a __Step-by-step example of a frontend change__.

I have forked the [Wingsuit Kickstarter](https://github.com/wingsuit-designsystem/wingsuit-kickstarter)
to provide a more user-friendly readme, enabling it to work as a primer.
That way even people who have no experience with [Storybook](https://storybook.js.org/) (and JS building tools) can see the
power with their naked eyes.

## Prerequisites

### Windows

- Windows 10 or 11 with [WSL2](https://docs.microsoft.com/en-us/windows/wsl/install) and [Ubuntu](https://ubuntu.com/) backend
- [Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/)

### Linux

- [Docker](https://docs.docker.com/engine/install/)

## Step-by-step installation

The following commands are to be run in Bash terminal in Linux or inside WSL2. You __do not need__ to clone
this repository, all the hard work is done by [Docksal](https://docksal.io/).

1. Install [Docksal](https://docksal.io/):
    ```
    bash <(curl -fsSL https://get.docksal.io)
    ```
2. Create a directory and enter into it:
    ```
    mkdir wingsuit-kickstarter
    cd wingsuit-kickstarter
    ```
3. Add the following entries to the `hosts` file:
    ```
    127.0.0.1 wingsuit-kickstarter.docksal
    127.0.0.1 storybook.wingsuit-kickstarter.docksal
    ```
4. (Windows WSL2 only) Set proxy:
    ```
    fin config set --global DOCKSAL_VHOST_PROXY_IP=127.0.0.1
    fin system reset vhost-proxy
    ```
5. Install and run [Wingsuit Kickstarter](https://github.com/wingsuit-designsystem/wingsuit-kickstarter) (it takes around 5 minutes on average PC):
    ```
    fin rc -T composer create-project wingsuit-designsystem/wingsuit-kickstarter wingsuit-kickstarter --stability dev --no-interaction
    cd wingsuit-kickstarter && fin init
    ```
6. Run [Storybook](https://storybook.js.org/):
    ```
    cd docroot/themes/custom/wingsuit && fin exec yarn dev:storybook:docksal
    ```
7. Open websites in your browser:
    - http://wingsuit-kickstarter.docksal
    - http://storybook.wingsuit-kickstarter.docksal

## Step-by-step for a frontend developer

As a frontend developer, you have installed only [Storybook](https://storybook.js.org/) (i.e. http://storybook.wingsuit-kickstarter.docksal).
Open a new project in your favourite IDE at `docroot/themes/custom/wingsuit/source`. That is
your root.

1. Now change the template `default/patterns/04-templates/basic/basic.twig`. This is a template for
the Basic page content type. Add a single line 31. `<div style="color: blue">Under content</div>` as in the code below:
    ```
      ...
      <div>{{ content }}</div>
      <div style="color: blue">Under content</div>
    {% endblock %}
    ...
    ```

Thanks to the [Webpack library](https://webpack.js.org) You can immediately see the change at http://storybook.wingsuit-kickstarter.docksal/?path=/story/templates-basic--basic.
Your job is done, you have created a frontend template __which can be presented to the client__ immediately
inside [Storybook](https://storybook.js.org/).

## Step-by-step for Drupal developer

As a Drupal Developer, you need the provided frontend bundle to be integrated inside the Drupal theme.
Thanks to two contributed Drupal modules, [UI Patterns](https://www.drupal.org/project/ui_patterns) and [Wingsuit Companion](https://www.drupal.org/project/wingsuit_companion) this is done
almost automatically:

1. In the frontend root (`docroot/themes/custom/wingsuit/source`) generate Drupal templates, assets and styles using:
    ```
    fin exec yarn ws dev drupal
    ```
2. Clear the Drupal cache:
    ```
    fin drush cr
    ```

Now, at http://wingsuit-kickstarter.docksal/en/legal-notice you can see the change
introduced by the frontend developer. __Your work is done__.

## Switching off

```
fin system stop
```
