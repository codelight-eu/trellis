# Trellis, pre-configured for Codelight
This is a fork of the official Trellis repo, configured for Codelight's use. It should be kept more or less up to date with the official repo. *Note that this repository is public.*

For anything not covered in the quick start guide below, see the [official Trellis docs](https://roots.io/trellis/docs/installing-trellis/) or the [official repo](https://github.com/roots/trellis).

Instructions below assume you're running a Windows machine.

## Structure
```shell
dev/              # → Root folder for all projects
├── trellis/      # → Your clone of this repository
└── site1/        # → A Bedrock-based WordPress site
    └── web/
        ├── app/  # → WordPress content directory (themes, plugins, etc.)
        └── wp/   # → WordPress core (don't touch!)
└── site2/        # → A Bedrock-based WordPress site
    └── web/
        ├── app/  # → WordPress content directory (themes, plugins, etc.)
        └── wp/   # → WordPress core (don't touch!)
└── site3/        # → A Bedrock-based WordPress site
    └── web/
        ├── app/  # → WordPress content directory (themes, plugins, etc.)
        └── wp/   # → WordPress core (don't touch!)
```

## Quick start
### Set up Trellis
_Note: The instructions here prefix Trellis folder with an underscore. This differs from the official docs._
1. Ensure you've installed all requirements listed [here](https://roots.io/trellis/docs/installing-trellis/)
2. In Windows, open git bash & create a new root directory:  
`$ mkdir dev && cd dev`
3. Clone codelight/trellis:  
`$ git clone --depth=1 git@github.com:codelight-eu/trellis.git _trellis`
4. Clone codelight/bedrock:  
`$ git clone --depth=1 git@github.com:codelight-eu/bedrock.git [SITENAME] && rm -rf [SITENAME]/.git`
5. Make a copy of the local environment config folder example (will not be stored in the repo):  
`$ cp -r _trellis/group_vars/development.example _trellis/group_vars/development`
6. Configure your WordPress site in `_trellis/group_vars/development/wordpress_sites.yml` and in `_trellis/group_vars/development/vault.yml` (use .local instead of .dev)
7. Run `$ cd _trellis && vagrant up`
8. Run `$ vagrant ssh` to access your new shiny box via SSH

### Create another synced folder and add our required tools
1. In Windows, clone our composer repo into the auto-generated tools folder:  
`$ cd dev/_tools && git clone git@github.com:codelight-eu/composer-global.git composer`
_todo: add a script to make this happen automagically?_

### Add a new local site
1. In Windows, open git bash & go to your root directory: `$ cd dev`
2. Clone codelight/bedrock:  
`$ git clone --depth=1 git@github.com:codelight-eu/bedrock.git [NEW_SITENAME] && rm -rf [NEW_SITENAME]/.git`
3. Configure your WordPress site in `_trellis/group_vars/development/wordpress_sites.yml` and in `_trellis/group_vars/development/vault.yml` (use .local instead of .dev)
4. Run `$ cd _trellis && vagrant provision`

### Update plugins via composer
While Composer works on both the host (Windows) machine and the guest (Vagrant) machine, you'll probably want to run `composer install` from inside the Vagrant box. Many of the composer packages used have specific requirements for various PHP components, which might be missing from your Windows machine. Running `composer install` from inside Vagrant bypasses that problem.

### Build the theme
Building the theme should also be done on the host (Windows) machine. Go to the theme folder and run `yarn run build:production`

## Documentation
### Global composer file
Codelight's private repositories are stored in our global composer.json file, which is merged to every site's own composer.json using [Composer Merge Plugin](https://github.com/wikimedia/composer-merge-plugin). If you need to add a new private or github-based repository, please add it to composer.global.json located in `dev/_tools/composer/composer.global.json`, commit and push.

### Development
When you install a plugin from github via composer, it comes with the git repo set up. In most cases, you should be able to use that same repository for development and push your changes back up to github.

### Local config
In order to override the default config with local values, create a file called vagrant.local.yml in trellis/ directory. Values defined in this file will override the values defined in vagrant.default.yml. For example, to increase the RAM assigned to the virtual machine, add the following line to vagrant.local.yml
```
vagrant_memory: 2048
```
