# Chop

Very minimal SSG generator based on Liquid-based templates. Written in ~300 lines of spaghetti JavaScript, utilizing the new Bun runtime.

## Content

Content is written in [CommonMark](https://commonmark.org/) Markdown. Chop also supports [Front Matter](https://jekyllrb.com/docs/front-matter/), to define variables that can later be used in your post, or in your templates.

## Templates

Templates are placed in the `templates` directory. The `templates` directory should contain subdirectories for each desired output (in my example I have `web`, `rss`, `gemini`).

The following two files from the subdirectory will be used:

- `index.*`: the file that will be used for content files that have `index` as basename. Meant as an index, listing pages/posts. If it doesn't exist, the `default.*` template will be used.
- `default.*`: the template that will be used for all other content files.

After building, the assembled files will be put in a `dist` directory. It also contains a subdirectory for every output.

### Global variables

Global variables can be defined in the optional `config.yaml` file. Variables defined here are available in templates.

I'm working on the ability to use variables in content.

### Special variables

These variables have special behavior, if specified:

- `site.prefix`: if specified in global config, all (relative) URLs (including those in content) will be prefixed with this string.

- `path`: if specified in the front matter of a post, it will be used to generate the final path. It is relative to the site root (aka the directory the site gets put in). Gets prefixed with `site.prefix`.

### Builtin variables

The following variables are exposed by default:

- `path`: the path of the final file, relative to the root of the site, includes the `site.prefix`.
- `path_unprefixed`: same as `path`, but without prepending the `site.prefix`.
- `content`: the plain CommonMark content.
- `content_rendered`: the HTML or gemtext rendered variant of the content.

In the case of an index, there is one more variable:

- `pages`: a variable that contains a list of pages. A page is an object that contains the frontmatter from the corresponding content file and the special variables listed above.

## Static files

Chop supports two types of static files:

- **Output-dependent static files**:  
  These are different for each template. The stylesheet, for example. Output-dependent files are placed inside the `static` folder in the desired output directory (`templates/OUTPUT/static`).

- **Global static files**:  
  These are the same for every output. Images used in posts, for example. Global files are placed inside the `static` folder in the base directory.

Images will be scaled down to a maximum resolution and then compressed. CSS and JavaScript are minified.

## Renderers

Depending on the extension of the template, your content will be translated either to HTML (`.html` or `.xml`), gemtext (`.gmi`) or kept as-is (everything else).

## Example configuration

Here's the folder structure for a simple blog:

```tree
~/blog
|- static
   |- images
      |- example.jpg
      |- avatar.png
|- templates
   |- web
      |- static
         |- main.css
      |- index.html
      |- default.html
   |- rss
      |- index.xml
|- config.yaml
|- hello-world.txt
|- index.txt
```
