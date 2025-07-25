<!-- omit in toc -->
# slidev-esp-template

This project is a template for [Slidev](https://github.com/slidevjs/slidev) presentations recommended for use at [Espressif](https://www.espressif.com/).

You can jump to:

- [History](#history)
- [Installation and configuration](#installation-and-configuration)
  - [Software dependencies](#software-dependencies)
  - [Template setup](#template-setup)
- [Usage](#usage)
  - [Examples of Slidev presentations](#examples-of-slidev-presentations)
- [Learn more about Slidev](#learn-more-about-slidev)
  - [Quick tips](#quick-tips)
- [Addons](#addons)
  - [Asciinema](#asciinema)
    - [Window resizing issue](#window-resizing-issue)
  - [Slide syncing](#slide-syncing)
    - [Troubleshooting](#troubleshooting)
  - [Poll and Quiz](#poll-and-quiz)
- [Layout and appearance](#layout-and-appearance)
  - [Visual layout settings](#visual-layout-settings)
  - [Update logo](#update-logo)

## History

The template is based on the Slidev [Getting Started](https://sli.dev/guide/) project with the following changes:

- **Theme**: instead of `seriph`, it uses the `default` theme.
- **Addons**: it includes
  - [@olzhas-adiyatov/slidev-addon-asciinema](https://www.npmjs.com/package/@olzhas-adiyatov/slidev-addon-asciinema)
  - [slidev-addon-sync](https://github.com/Smile-SA/slidev-addon-sync)
  - [Poll and Quiz](https://github.com/Smile-SA/slidev-component-poll)
- **Layout**: Espressif logo added.
- **Slide content**:
  - Replaced Slidev Getting Started title page with a usual Espressif title page
  - Changed guidelines in *Themes*
  - Added a slide *Asciinema* describing this plugin
  - Added a slide *Poll and Quiz* describing this plugin


## Installation and configuration

You can use your OS native package manager to install the dependencies, however, it is recommended to use `nvm` -- Node.js version manager.

The following instructions are provided in detail, as parts of the Node.js ecosystem may not be immediately intuitive to some users. These steps have been tested on Linux only. The general approach should be similar on other operating systems with some possible adjustments.

If you run into major problems installing and configuring the dependencies, feel free to open an issue — we’ll do our best to update the instructions accordingly.

### Software dependencies

Slidev requires `Node.js`, which can be conveniently installed using [nvm](https://github.com/nvm-sh/nvm?tab=readme-ov-file#installing-and-updating):

```sh
# Install nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.nvm/nvm.sh
# Install Node.js (LTS version includes npm)
nvm install --lts
```


### Template setup

To create your presentation:

1. On the GitHub repo page, click **Use this template** in the top-right corner.
    - This creates a new repo under your GitHub account with the contents of `slidev-esp-template`.
2. Clone your new repo:
    ```sh
    git clone https://github.com/YOUR_USERNAME/slidev-esp-template.git
    cd ~/path/to/slidev-esp-template/
    ```
3. Install project dependencies:
    ```sh
    npm install
    ```
4. (Optional but recommended) Set up your environment to use Slidev globally:
    ```sh
    # Install pnpm globally (recommended over npm for faster installs and disk efficiency)
    npm install -g pnpm
    # Create a global bin directory and add it to your shell's PATH
    pnpm setup
    # Restart your shell
    # Install the Slidev CLI globally
    pnpm add -g @slidev/cli
    ```
    💡 If you prefer not to install anything globally, you can run Slidev using `npm run dev`.

## Usage

To start the slide show:

- Run `slidev`
- Visit <http://localhost:3030>

Edit the [slides.md](./slides.md) to see the changes.

During your talk, you can:

- Use a websocket in the LAN by running `slidev --remote=<password> --host=0.0.0.0`<br>
OR
- Use a workflow to build and [Deploy Slidev to GitHub Pages](https://github.com/igrr/esb24/blob/main/.github/workflows/deploy.yml)


### Examples of Slidev presentations

- Espressif:
  - [DevCon23 - Developing, Publishing, and Maintaining Components for ESP-IDF](https://www.youtube.com/watch?v=D86gQ4knUnc) (YouTube)
  - [ESP-IDF and Tools Overview](https://github.com/igrr/esb24/) (GitHub)
- [Slidev Showcases](https://sli.dev/resources/showcases)

## Learn more about Slidev

To get started, see:

- [Syntax Guide](https://sli.dev/guide/syntax)
- [Directory Structure](https://sli.dev/custom/directory-structure)
- [User Interface](https://sli.dev/guide/ui)

This is part of extensive Slidev [documentation](https://sli.dev/).

### Quick tips

The most basic and important features:

- **VS Code extension**: If you use VS Code, you can install [Slidev for VS Code](https://sli.dev/features/vscode-extension.html) extension to improve your experience using Slidev.
- **Include static files**: If you have static files, such as images or diagrams, place them in the `public/` folder. You can then reference them using `src="/image.png"`. For example, to include the image `public/myimage.jpeg` on your slide, use `src="/myimage.png"`.
- **Embed external code blocks**: If you need to include code blocks, place files containing code in the `snippets/` folder. Then:
  - Mark the regions to include:
    ```ts
    // #region snippet
    ...
    // #endregion snippet
    ```
  - Include the regions:
    ```ts
    <<< @/snippets/external.ts#snippet
    ```
- **Arrange content in slides**: You can save time arranging content in your slides using preset [layouts](https://sli.dev/guide/layout) in your slide frontmatter:
  ```markdown
  ---
  layout: two-cols
  ---
   ```
- **Simplify content reuse**: If you plan to reuse certain slides or sections in other Slidev presentations, you can place this content in separate markdown files inside the `pages/` folder, for example `pages/content.md`. To include them into your main `slides.md`, use:
  ```markdown
  ---
  src: ./pages/content.md
  hide: false
  ---
  ```
  The parameter `hide` conveniently helps to show or hide the included slides.
- **Create tree diagrams**: Though not a Slidev feature, the tool [tree.nathanfriend.com](https://gitlab.com/nfriend/tree-online) can be very helpful in visualizing folder trees, decision trees, etc., in your slides:
  ```markdown
  📂 my-project/
  ├── 📝 README.md
  └── 📁 files/
  ```
- **Create diagrams as code**: Slidev supports the following tools
  - [Mermaid](https://mermaid.js.org/intro/) ([live editor](https://mermaid.live/))
  - [PlantUML](https://plantuml.com/) ([live editor](https://editor.plantuml.com/))
- **Visualize data with D3**: You can use [D3.js](https://d3js.org/) to create complex diagrams and visualizations by creating a [component](https://sli.dev/guide/component).
- **Use TailwindCSS for advanced formatting**: TailwindCSS is already used in Vite (upon which Slidev is built). It can help create more advanced layouts, e.g., [columns](https://tailwindcss.com/docs/columns) or [grid](https://tailwindcss.com/docs/grid-template-columns).


## Addons

### Asciinema

You can create an Asciinema cast following the [Getting Started](https://docs.asciinema.org/getting-started/) guide.

The asciinema player's [options](https://docs.asciinema.org/manual/player/options/) can be included via `:playerProps`:

```markdowm
<Asciinema src="casts/demo.cast" :playerProps="{speed: 2, rows: 13}"/>
```

See a usage example in [slides.md](./slides.md#asciinema) > *Asciinema*.

#### Window resizing issue

By default, the header of the generated `.cast` file embeds the `width` and `height` parameters:

```sh
{"version": 2, "width": 130, "height": 24, "timestamp": 1710834119, <...>}
```

When you hit the _play_ button, the presence of `width` and `height` can result in unnecessary window resizing. To avoid window resizing, remove these parameters:

```sh
{"version": 2, "timestamp": 1710834119, <...>}
```

### Slide syncing

To enable slide syncing, run:

```sh
slidev --remote=<password>
```

This command will return the links for the presenter and attendees.


#### Troubleshooting

If slide syncing doesn't work, make sure that:

- The dev server (`slidev --remote`) is running somewhere (usually the presenter's device).
- The presenter is using the presenter link.
- The presenter's and attendee's devices are connected to the same network.
- The device's OS does not block access to the LAN due to the use of VPN clients, etc.


### Poll and Quiz

See a usage example in [slides.md](./slides.md#poll-and-quiz) > *Poll and Quiz*.

## Layout and appearance

### Visual layout settings

- **Page numbers**: To add page numbers, open the [global-bottom.vue](./global-bottom.vue) file and uncomment its contents.
- **Color scheme**: To change the color scheme, in the `slides.md` YAML frontmatter, update the parameter `colorSchema`. The default value is `light`, but you can update it to `auto` or `dark`.
- **Aspect ratio and canvas size**: To change the configuration, in the `slides.md` YAML frontmatter, update the values of `aspectRatio` and `canvasWidth`.

### Update logo

If the slides are for an event that has its own logo (for example, DevCon25), consider replacing the standard Espressif logo with the event-specific logo.

To update the logo, follow these steps:

1. Choose the logo in the `public/` folder. If your event's logo is not there, add it and consider adding it to the upstream template repo as well.
2. In the `slides.md` YAML frontmatter, find the parameter `favicon` and add the path to the new logo.
3. Preview the slides to make sure the logo is clearly visible and works well with the slide background color.
