# dify_code_node_get_image
如何让 dify工作流的 code 节点拿到图片的信息
本仓库为AI带路党Pro视频准备
#### 修改api源码
1. api/services/file_service.py 中 FileService类upload_file函数中,
```
source_url = '/app/api/storage/' + file_key if not source_url else source_url
# save file to storage
storage.save(file_key, content)
```
#### 修改沙盒配置
1. 修改 docker/volumes/sandbox/conf/config.yaml
  ```
   allowed_syscalls: [0,1,2,3,4,5,6,7,8,9,14,15,21,22,25,26,29,30,31,32,33,34,35,38,39,43,44,45,46,56,57,61,62,63,64,71,72,79,80,94,98,101,131,132,134,135,139,144,146,172,215,222,226,318,334,307,262,16,8,217,1,3,257,0,202,9,12,10,11,15,25,105,106,102,39,110,186,60,231,234,13,16,24,273,274,334,228,96,35,291,233,230,270,201,14,131,318,56,258,83,41,42,49,50,43,44,45,51,47,52,54,271,63,46,307,55,5,72,138,7,281]
   ```
2. 
