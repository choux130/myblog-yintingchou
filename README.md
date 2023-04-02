This personal website use a Hugo theme called [harbor](https://github.com/matsuyoshi30/harbor) with some of customized adjustments. 

Here are the customizations, 
* Enable [KaTex](https://katex.org/) by following this [harbor tutorial](https://matsuyoshi30.net/harbor/2019/03/08/math-typesetting/). 
    1. Create a file under `~/themes/harbor/layouts/partials/math.html`.
        ```html
        <!-- html file under ~/themes/harbor/layouts/partials/math.html -->

        </script>
        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.4/dist/katex.min.css" integrity="sha384-vKruj+a13U8yHIkAyGgK1J3ArTLzrFGBbBc0tDp4ad/EyewESeXE/Iv67Aj8gKZ0" crossorigin="anonymous">
        <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.4/dist/katex.min.js" integrity="sha384-PwRUT/YqbnEjkZO0zZxNqcxACrXe+j766U2amXcgMg5457rve2Y7I6ZJSm2A0mS4" crossorigin="anonymous"></script>
        <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.4/dist/contrib/auto-render.min.js" integrity="sha384-+VBxd3r6XgURycqtZ117nYw44OOcIax56Z4dCRWbxyPt0Koah1uHoK0o4+/RRE05" crossorigin="anonymous"
            onload="renderMathInElement(document.body);"></script>
        ```
    2. Add the following code in `~/themes/harbor/layouts/_default/single.html`.
        ```html
        {{ if or .Params.math .Site.Params.math }}
            {{ partial "math.html" . }}
        {{ end }}
        ```
* Instead of using Disqus, I use [utterances](https://utteranc.es/) as the comment tool which sync comments to github issues. 
    1. Create a file under `~/themes/harbor/layouts/partials/utterances.html`
        ```html
        <!-- html file under ~/themes/harbor/layouts/partials/utterances.html -->

        <script src="https://utteranc.es/client.js"
            repo="choux130/myblog-yintingchou"
            issue-term="title"
            label="comments 💬"
            theme="github-light"
            crossorigin="anonymous"
            async>
        </script>
        ```
    2. Edit the following code block in `~/themes/harbor/layouts/_default/single.html`.
        ```html
        {{ if  (not (isset .Params "disable_comments")) }}
        <!-- {{ partial "disqus.html" . }} -->
        {{ partial "utterances.html" . }}
        {{ end }}
        ```
    * [Goodbye, Disqus! Hello, Utterances!](https://masalmon.eu/2019/10/02/disqus/)
* Change the page Archives's layout by editing the following code block in `~/themes/harbor/layouts/archives/single.html`
    ```html
    <table class="archives">
        <!-- <tbody>
        {{ range (where (where .Site.Pages ".Section" "post") "Kind" "page") }}
            <tr>
            <td>
                {{ .Date | dateFormat "2006" }}&nbsp;{{ .Date | dateFormat "Jan" }}&nbsp;{{ .Date | dateFormat "2" }}
            </td>
            <td>
                <p>
                <a href="{{ .Permalink }}" title="{{ .Title }}"
                    >{{ .Title }}</a
                >
                </p>
            </td>
            </tr>
        {{ end }}
        </tbody> -->

        {{ $updated_year := .Site.LastChange.Format "2006" }}
        {{ range (where (where .Site.Pages ".Section" "post") "Kind" "page").GroupByDate "2006" -}}
        <tbody>
            {{ if eq .Key $updated_year }}
            <tr>
                <td  style="padding-left: 0px; padding-top:0px;">
                <strong style="font-size: 25px;">{{ .Key }}</strong>
                </td>
            </tr>
            {{ else }}
            <tr>
                <td  style="padding-left: 0px; padding-top:15px;">
                <strong style="font-size: 25px;">{{ .Key }}</strong>
                </td>
            </tr>
            {{ end }}
            
            {{ range .Pages }}
            <tr>
                <td>
                {{ .Date | dateFormat "Jan" }}&nbsp;{{ .Date | dateFormat "2" }}
                </td>
                <td>
                <p>
                    <a href="{{ .Permalink }}" title="{{ .Title }}"
                    >{{ .Title }}</a
                    >
                </p>
                </td>
            </tr>
            {{ end }}
        </tbody>
        {{ end }}
    </table>    
    ```
* Move tag's location in the post to the top by changind the code in `~/themes/harbor/layouts/_default/single.html`.
    ```html
    <article class="article" class="blog-post">
      <!-- {{ partial "toc.html" . }} -->

      {{ if or .Params.math .Site.Params.math }}
        {{ partial "math.html" . }}
      {{ end }}

      {{ if .Params.tags }}
        <div class="blog-tags">
          {{ range .Params.tags }}
            <a
              href="{{ "" | absLangURL }}tags/{{ . | urlize }}{{ if $.Site.Params.uglyurls }}.html{{ else }}/{{ end }}"
              >{{ . }}</a
            >&nbsp;
          {{ end }}
        </div>
      {{ end }}

      {{ partial "toc.html" . }}

      {{ if .Site.Params.enableBottomNavigation }}
        {{ partial "bottomnav.html" . }}
      {{ end }}
    </article>
    ```
* Change the favicons, 
    * `~/themes/harbor/static/favicon.ico`
    * `~/themes/harbor/static/images/icon.png`
* Change the `intro-header` font size from 50px to 40px in `~/themes/harbor/assets/css/main.css`, 
    ```css
    .intro-header [class$="-heading"] h1 {
        margin-top: 0;
        padding-top: 0;
        font-size: 40px;
    }
    ```