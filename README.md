# What is this?
This is the repo that I'm using to host the files for my site, which consist of a portfolio, and a wiki. The domain that this is all hosted on is https://entering.theworkpc.com and uses Github Pages for hosting.  
The root (`/`) of the site runs my porfolio, while the wiki runs on the subpath of `/wiki`
## Folder heirarchy and technologies used
The heirarchy is simple and follows as this
```sh
./
    portfolio/ # folder for my portfolio
    wiki/ # folder for my wiki
```  
For the portfolio, it is currently an Angular site but that will probably change in the future. 
For the wiki, it is made using the Astro Starlight documentation framework. 
## 🧞 Commands for running the wiki
First, do `cd wiki` and then you can run the commands below
All commands are run from the root of the project, from a terminal:

| Command                   | Action                                           |
| :------------------------ | :----------------------------------------------- |
| `npm install`             | Installs dependencies                            |
| `npm run dev`             | Starts local dev server at `localhost:4321`      |
| `npm run build`           | Build your production site to `./dist/`          |
| `npm run preview`         | Preview your build locally, before deploying     |
| `npm run astro ...`       | Run CLI commands like `astro add`, `astro check` |
| `npm run astro -- --help` | Get help using the Astro CLI                     |

## 👀 Want to learn how to use Starlight?

Check out [Starlight’s docs](https://starlight.astro.build/), read [the Astro documentation](https://docs.astro.build), or jump into the [Astro Discord server](https://astro.build/chat).
