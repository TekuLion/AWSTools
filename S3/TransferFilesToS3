#!/bin/bash
today=$(date "+%Y%m%d")

# ファイルの存在チェック（ファイルパスは適宜変更）
ls -l /file_path/${today}.zip
file_check=`echo $?`

if [ ${file_check} -eq 0 ] ; then
  # ローカルファイルをS3のバケットに送る（バケット名は適宜変更）
  aws s3 cp /file_path/${today}.zip s3://bucket_name/
fi

# 過去10日のバックアップファイルを削除
targetDay=`date --date "${today} 10 days ago" +%Y%m%d`

# ローカルに削除対象のファイルが存在するかチェック
ls -l /file_path/${today}.zip
local_backup_check=`echo $?`
if [ ${local_backup_check} -eq 0 ] ; then
  rm -f /file_path/${targetDay}.zip
fi

# バケット内に削除対象のファイルが存在するかチェック
aws s3 ls s3://bucket_name/${targetDay}.zip
s3_backup_check=`echo $?`
if [ ${s3_backup_check} -eq 0 ] ; then
  aws s3 rm s3://bucket_name/${targetDay}.zip
fi
