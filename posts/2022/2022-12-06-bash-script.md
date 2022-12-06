# A simple Bash Script

```bash
#!/usr/bin/env bash

for jar in $(find . -name '*.jar'); do
  pom_file=$(unzip -Z1 $jar | grep 'pom.xml$')
  #echo $pom_file
  unzip -p $jar $pom_file | grep -i -C 10 '<version>.*\-snapshot</version>'
  if [ $? -eq 0 ]
  then
    echo $pom_file
  fi
done
```
