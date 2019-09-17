---
title: Linux查找替换
tags: linux

---

**批量查找并替换**

 find . | grep BuildConfig.groovy | xargs perl -pi -e 's|m:/workspace/m2|./m2|g'

**批量查找并替换，使用正则**

find . | grep BuildConfig.groovy | xargs perl -pi -e 's|mavenLocal(.)*./m2"\)|mavenLocal("/opt/source/m2")|g'

**批量查找文件内容**

find . | grep BuildConfig.groovy | xargs grep -F 'mavenLocal'

**批量替换windows换行符为linux换行符**

find . | grep BuildConfig.groovy | xargs sed -i 's/\r//'