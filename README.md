## Домашнее задание к занятию «2.4. Инструменты Git»

**1. Найдите полный хеш и комментарий коммита, хеш которого начинается на `aefea`.**  
Полный хеш: `aefead2207ef7e2aa5dc81a34aedf0cad4c32545`  
Комментарий: `Update CHANGELOG.md`  
```
$ git log aefea --pretty=format:"%H %s " -1  
aefead2207ef7e2aa5dc81a34aedf0cad4c32545 Update CHANGELOG.md
```
**2. Какому тегу соответствует коммит `85024d3`?**  
Тегу: `v0.12.23`
```
$ git tag --list --points-at 85024d3  
v0.12.23
```

**3. Сколько родителей у коммита `b8d720`? Напишите их хеши.**   
Два родителя, их хэши:  
`56cd7859e05c36c06b56d013b55a252d0bb7e158`  
`9ea88f22fc6269854151c571162c5bcf958bee2b`  
```
$ git rev-parse b8d720^@
56cd7859e05c36c06b56d013b55a252d0bb7e158
9ea88f22fc6269854151c571162c5bcf958bee2b
```

**4. Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами  `v0.12.23` и `v0.12.24`.**

```
$ git log v0.12.23..v0.12.24^ --pretty=format:"%H %s"  
b14b74c4939dcab573326f4e3ee2a62e23e12f89 [Website] vmc provider links  
3f235065b9347a758efadc92295b540ee0a5e26e Update CHANGELOG.md  
6ae64e247b332925b872447e9ce869657281c2bf registry: Fix panic when server is unreachable  
5c619ca1baf2e21a155fcdb4c264cc9e24a2a353 website: Remove links to the getting started guide's old location
06275647e2b53d97d4f0a19a0fec11f6d69820b5 Update CHANGELOG.md  
d5f9411f5108260320064349b757f55c09bc4b80 command: Fix bug when using terraform login on Windows
4b6d06cc5dcb78af637bbb19c198faff37a066ed Update CHANGELOG.md  
dd01a35078f040ca984cdd349f18d0b67e486c35 Update CHANGELOG.md  
225466bc3e5f35baa5d07197bbc079345b77525e Cleanup after v0.12.23 release  
```


**5. Найдите коммит в котором была создана функция `func providerSource`, ее определение в коде выглядит 
так `func providerSource(...)` (вместо троеточего перечислены аргументы).**  
Функция была создана в коммите: `8c928e83589d90a031f811fae52a81be7153e82f`

```
$ git log -S "func providerSource" --pretty=format:"%H %s %as"  
5af1e6234ab6da412fb8637393c5a17a1b293663 main: Honor explicit provider_installation CLI config when present 2020-04-21  
8c928e83589d90a031f811fae52a81be7153e82f main: Consult local directories as potential mirrors of providers 2020-04-02   
```

**6. Найдите все коммиты в которых была изменена функция `globalPluginDirs`.**    
Находим файл, содержащий функцию `globalPluginDirs`, это файл: `plugins.go`    
```
$ git grep globalPluginDirs 
commands.go:            GlobalPluginDirs: globalPluginDirs(),
commands.go:    helperPlugins := pluginDiscovery.FindPlugins("credentials", globalPluginDirs())
internal/command/cliconfig/config_unix.go:              // FIXME: homeDir gets called from globalPluginDirs during init, before
plugins.go:// globalPluginDirs returns directories that should be searched for
plugins.go:func globalPluginDirs() []string {
```
Затем, находим коммиты:
```
$ git log  -L:globalPluginDirs:plugins.go --pretty=format:"%h %s %as" | egrep "^([a-h0-9]){8,}"
78b122055 Remove config.go and update things using its aliases 2020-01-13
52dbf9483 keep .terraform.d/plugins for discovery 2017-08-09
41ab0aef7 Add missing OS_ARCH dir to global plugin paths 2017-08-09
66ebff90c move some more plugin search path logic to command 2017-05-03
8364383c3 Push plugin discovery down into command package 2017-04-13
```

**7. Кто автор функции `synchronizedWriters`?**  
Автор: `Martin Atkins`
```
$ git log -S "synchronizedWriters" --pretty=format:"%an %as %h %s"
James Bardin 2020-11-30 bdfea50cc remove unused
James Bardin 2020-10-21 fd4f7eb0b remove prefixed io
Martin Atkins 2017-05-03 5ac311e2a main: synchronize writes to VT100-faker on Windows
```


---
***


## Домашнее задание 4


## Домашнее задание 3


## Домашнее задание 2

В каталоге terraform и его подкаталогах будут проигнорированы файлы:  
crash.log  
override.tf  
override.tf.json  
.terraformrc  
terraform.rc  

В каталоге terraform и его подкаталогах будут проигнорированы файлы по маске:  
`*`.tfstate  
`*`.tfstate.`*`  
`*`.tfvars  
`*`_override.tf  
`*`_override.tf.json  

В каталоге terraform будут проигнорированы все файлы из подкаталогов с именем `.terraform`
