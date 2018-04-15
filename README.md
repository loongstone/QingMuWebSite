
 **[点此查看网站效果](http://qm.itsky.top)**
## 使用说明

通过``` hugo new /xxx/index.md ```创建新页面,通过Menu配置main参数,显示在首页菜单中,通过配置main2参数,显示在首页意外的菜单中
通过menuSuper : [MenuName],与menu : menuName ,设置二级页中的导航.
一共ch创建并使用了三个模板:index.html,list.html和single.html,除站点index页面外,其他页面都可以通过*.md文件创建\修改网站页面,并通过参数配置菜单导航.

## 重构说明:
- 创建 list.html ,根据Page中定义的Menu生成菜单.
- 创建 single.html ,填充md文件作为内容
- 因对网页中的 CSS,JS 不够了解,这部分基本使用了原网站的代码,如果需要适配移动端屏幕,需要重构这部分.
- 原网站中很多部分通过 hugo 实现并不合适,比如页面内容图文混排,单纯 MarkDown 方式无法实现,需要内嵌HTML,但这又失去了 hugo 的简洁性,重构网站尽量还原原有内容.

```
        {{ $currentPage := . }}
        {{ $currentMenu := .Params.menuSuper }} 
        {{ range .Site.Menus }} 
            {{range .}}
                {{if eq .Menu $currentMenu}}
                    {{if or ($currentPage.IsMenuCurrent $currentMenu .) ($currentPage.HasMenuCurrent $currentMenu .)}}
                        <li class="active">{{.Title}}</li>

                    {{else}}
                        <li>
                            <a href={{.URL}}>{{.Title}}</a>
                        </li>
                    {{ end }}  
                {{end}}
            {{end}}
        {{ end }}
```

```
{{ $currentPage := . }}
            {{ range .Site.Menus.main }}
                {{ if .HasChildren }}
                    <li class="{{ if $currentPage.HasMenuCurrent "main" . }}active{{ end }}">
                        <a href="#">
                            {{ .Pre }}
                            <span>{{ .Name }}</span>
                        </a>
                    </li>
                    <ul class="sub-menu">
                        {{ range .Children }}
                            <li class="{{ if $currentPage.IsMenuCurrent "main" . }}active{{ end }}">
                                <a href="{{ .URL }}">{{ .Name }}</a>
                            </li>
                        {{ end }}
                    </ul>
                {{ else }}
                    <li>
                        <a href="{{ .URL }}">
                            {{ .Pre }}
                            <span>{{ .Name }}</span>
                        </a>
                    </li>
                {{ end }}
            {{ end }}
```
 
 提取公共HTML ,JS代码