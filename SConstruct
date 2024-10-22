# -*- coding: utf-8 -*-
# ===================================================================
#
# Copyright © 2023 China Mobile IOT. All rights reserved.
#
# Date:     2023/02/24
# Author:   zhangxw
# Modify:   by zhangxw@2023/6/16，适配ML307R平台
# Function: scons工程文件
#
# ===================================================================


import os
import sys

# scons脚本路径添加至系统
sys.path.append(os.path.realpath(os.path.join('tools', 'scons')))
from EnvironConfig import *
from UtilsTools import *

# 系统环境配置
op_sys = Platform()
print("Operation system: %s" % op_sys)

env = Environment(
    ENV={'PATH': os.environ['PATH']},
    tools=['default', 'gcc', 'g++', 'gas', 'ar', 'gnulink'],
    toolpath=['tools/scons'],
    PROGSUFFIX='.elf',
    BUILD_DIR=PathConfig.BUILD_DIR,
)
env.PrependENVPath('PATH', PathConfig.GUN_PATH)  # GUN工具路径
env.PrependENVPath('PATH', PathConfig.ABOOT_PATH)  # aboot工具路径
# print(env['ENV']['PATH'])

# 读取命令行参数
# debug = ARGUMENTS.get('debug', 0)


# 生成工程环境
root_dir = os.path.join(Dir('#').abspath)
VersionConfig.init_module_version(env, ARGUMENTS)
install_tools(env)  # 安装工具
env['IMAGE_DIR'] = os.path.join(PathConfig.IMAGE_DIR, env['MODULE_SUB_NAME'])
env['OBJECT_DIR'] = os.path.join(PathConfig.OBJECT_DIR, env['MODULE_SUB_NAME'])
env['BASELINE_DIR'] = os.path.join(PathConfig.BASELINE_DIR, env['MODULE_SUB_NAME'])
env['LDS_SRC'] = os.path.join(PathConfig.LINK_DIR, env['MODULE_SUB_NAME'], 'app_' + env['MODULE_SUB_NAME'].lower() + '.ld')


# 链接配置
# ①链接器文件生成
# ②链接器，map文件配置
target_map = env['TARGET_NAME'] + '.map'
env.AppendUnique(LINKFLAGS=['-L%s' % env['IMAGE_DIR']])
env.AppendUnique(LINKFLAGS=['-T%s' % 'linker.lds'])
env.AppendUnique(LINKFLAGS=['-Wl,-Map=' + os.path.join(env['IMAGE_DIR'], target_map)])


Export('env', 'root_dir', 'target_map')
SConscript('SConscript', variant_dir=env['OBJECT_DIR'], duplicate=0)
