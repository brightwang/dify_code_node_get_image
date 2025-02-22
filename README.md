# dify_code_node_get_image
如何让 dify工作流的 code 节点拿到图片的信息
本仓库为AI带路党Pro视频准备[如何在dify的code节点中访问图片文件-dify二次开发
](https://www.bilibili.com/video/BV1BRPNeLEDv/?vd_source=e60241d08fb6eeea3c6a0e9196f65bc0)
#### 修改api源码
1. api/services/file_service.py 中 FileService类upload_file函数中,在storage.save之前新增代码
```
# 新增此行
source_url = '/app/api/storage/' + file_key if not source_url else source_url
# save file to storage
storage.save(file_key, content)
```
#### 修改沙盒配置
1. 修改 docker/volumes/sandbox/conf/config.yaml，为什么加这个请看 up 主之前沙盒的视频
  ```
   allowed_syscalls: [0,1,2,3,4,5,6,7,8,9,14,15,21,22,25,26,29,30,31,32,33,34,35,38,39,43,44,45,46,56,57,61,62,63,64,71,72,79,80,94,98,101,131,132,134,135,139,144,146,172,215,222,226,318,334,307,262,16,8,217,1,3,257,0,202,9,12,10,11,15,25,105,106,102,39,110,186,60,231,234,13,16,24,273,274,334,228,96,35,291,233,230,270,201,14,131,318,56,258,83,41,42,49,50,43,44,45,51,47,52,54,271,63,46,307,55,5,72,138,7,281]
   ```
#### 修改 docker-compose.yaml
1. api节点修改不用 image而是通过 build（为什么请参考 up 主往期视频）
```
api:
    build: ../api
    restart: always
```
2. 修改 sandbox 节点中的volumes
```
volumes:
      - ./volumes/sandbox/dependencies:/dependencies
      - ./volumes/sandbox/conf:/conf
      - ./volumes/app/storage:/var/sandbox/sandbox-python/usr/local/storage
```
3. 修改后分别执行 docker compose down、docker compose build、docker compose up。拉不到镜像，无法安装包等问题请自习挂代理等方式解决
#### 导入 仓库中 demo-upload.yml工作流进行测试
返回示例,result 为文件大小，url 为存储于 dify-api中的图片路径，在
```
{
  "result": 227683,
  "name": "test-3.png",
  "url": "/app/api/storage/upload_files/13e6c543-0996-4c9e-853f-5c023ee48c6d/d5d0aab0-02af-41ed-abab-bac2076f49fb.png"
}
```
