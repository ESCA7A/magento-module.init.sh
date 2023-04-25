# magento-module.init.sh
easy module deploy from magento 2 app


```bash
#!/bin/bash

currentDir=$PWD
echo $currentDir

echo -n 'vendor name: '
read VENDOR

echo -n 'module name: '
read MODULE

echo -n 'create composer json? <yes> or skip: '
read isCreateComposer

del="_"
modulexml=("<?xml version=\"1.0\"?>\n\n<config xmlns:xsi=\"http://www.w3.org/2001/XMLSchema-instance\" xsi:noNamespaceSchemaLocation=\"urn:magento:framework:Module/etc/module.xsd\">\n\t<module name=\"$VENDOR$del$MODULE\"/>\n</config>")
registration=("<?php\n\nuse Magento\Framework\Component\ComponentRegistrar;\n\nComponentRegistrar::register(ComponentRegistrar::MODULE, \"$VENDOR$del$MODULE\", __DIR__);")

mkdir -p src/app/code/$VENDOR/$MODULE/etc

echo -e $modulexml > src/app/code/$VENDOR/$MODULE/etc/module.xml

echo -e $registration > src/app/code/$VENDOR/$MODULE/registration.php

if [[ $isCreateComposer == 'yes' ]]
  then
    cd src/app/code/$VENDOR/$MODULE
    composer init
    cd $currentDir
    $PWD/bin/magento module:enable $VENDOR$del$MODULE
    $PWD/bin/magento s:up
  else
    $PWD/bin/magento module:enable $VENDOR$del$MODULE
    $PWD/bin/magento s:up
    cd $currentDir
    echo "module is created without composer json >:)\n"
fi
```
