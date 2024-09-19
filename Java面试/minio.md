# 分片上传

前端计算md5，上传给后端，后端通过比对md5，判断文件是否存在，存在就不传了，如果不存在

前端计算分片，发送请求给后端，后端返回给前端每一个分片的签名。

然后前端使用url签名上传，上传完后，调用合并接口，合并我们的分片。

如何觉得计算md5比较慢，可以直接前后两个切片+中间的切片的前中后6个字节。

https://minioplus.liuxp.me/guide/core/upload.html#%E5%88%86%E7%89%87%E4%B8%8A%E4%BC%A0

https://gitee.com/Gary2016/minio-upload

https://blog.csdn.net/qq_35222232/article/details/132915126?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ECtr-1-132915126-blog-135111891.235%5Ev43%5Epc_blog_bottom_relevance_base9&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ECtr-1-132915126-blog-135111891.235%5Ev43%5Epc_blog_bottom_relevance_base9&utm_relevant_index=2

https://blog.csdn.net/qq_34813456/article/details/135111891#:~:text=minio%E4%BC%9A%E5%AF%B9%E5%AE%98,%E5%88%86%E7%89%87%E4%BC%9A%E8%87%AA%E5%8A%A8%E5%88%A0%E9%99%A4%E3%80%82

https://blog.csdn.net/weixin_51603038/article/details/130408421

[这里说tomcat会默认分片，写的也挺好的](https://www.elibaron.com/arch/minio/minio-uploader.html#_1-1-%E4%BC%98%E5%8C%96%E6%96%B9%E6%A1%88)

minio合并文件的时候，etga是按照byte拼接的，而不是重新计算的。

断点续传很简单，计算分片是否存在就行了。

初始化 分片上传任务，获取uploadid

根据partnumber获取文件签名，然后前端直传

传完之后，后端根据每一个part，结合uploadid，进行整合


看了很多，大致的流程都是:
1. 前端计算md5，与后端进行比较，如果存在，则结束
2. 创建一个分片上传任务，得到uploadid
3. 根据前端传过来的分片数量，返回直传url
4. 前端通过直传url进行上传（reqParams里添加uploadId和partNumber）
5. 上传完之后调用合并接口进行合并
6. 后端根据uploadId和part数量，合并分片文件。

断点续传可以通过uploadid，得到上传文件的每一个part。一一比对就行了

好处：分片+断点上传，并且实时监控进度。默认5MB。

