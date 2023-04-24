 /virus/runtime_env 是一个文件，其内容为 develop 或 test 等配置，可以在 update.sh 脚本中添加相应的逻辑

 ```sh
 #!/bin/bash

# 根据 /virus/runtime_env 文件中的内容选择不同的 etc 目录
if [ -f "/virus/runtime_env" ]; then
  CONFIG_DIR="/virus/etc"
  RUNTIME_ENV=$(cat /virus/runtime_env)
  if [ "$RUNTIME_ENV" == "develop" ]; then
    CONFIG_DIR="./develop_etc"
  elif [ "$RUNTIME_ENV" == "test" ]; then
    CONFIG_DIR="./test_etc"
  fi
fi

# 下载更新文件
wget $UPDATE_URL

# 解压更新文件
tar -zxvf $UPDATE_FILE

# 替换相关文件
cp $UPDATE_DIR/bin/* $BIN_DIR/
cp $UPDATE_DIR/conf/* $CONFIG_DIR/

# 将 etc 目录修改为 ./etc
sed -i "s|$CONFIG_DIR|./etc|g" $BIN_DIR/*

# 重启相关服务
systemctl restart $SERVICE_NAME

```
