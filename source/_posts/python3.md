---
title: python 日志工具类
date: 2020-06-19 15:51:49
categories: python
tags: python io
---

```python
'''
@Description: 日志工具类
'''
import time
from loguru import logger
from pathlib import Path

project_path = Path.cwd()
log_path = Path(project_path, "logs")
t = time.strftime("%Y_%m_%d")

class Loggings(object):
    __instance = None
    logger.add(f"{log_path}/operation_log_{t}.log",format="{time:YYYY-MM-DD HH:mm:ss} | {level} | {message}", rotation="50MB", encoding="utf-8", enqueue=True,
               retention="10 days")

    def __new__(cls, *args, **kwargs):
        if not cls.__instance:
            cls.__instance = super(Loggings, cls).__new__(cls, *args, **kwargs)

        return cls.__instance

    def info(self, msg):
        return logger.info(msg)

    def debug(self, msg):
        return logger.debug(msg)

    def warning(self, msg):
        return logger.warning(msg)

    def error(self, msg):
        return logger.error(msg)


# 测试,只有在当前文件直接执行时，才会执行以下代码
loggings = Loggings()
if __name__ == '__main__':
    loggings.info("info 测试")
    loggings.debug("debug 测试")
    loggings.warning("warn 测试")
    loggings.error("error 测试")
        
```
