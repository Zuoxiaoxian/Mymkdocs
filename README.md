1、 进入目录
cd Mymkdocs

2、创建新的MkDocs项目
docker run --rm -it -v $(pwd):/docs squidfunk/mkdocs-material new .

3、构建和运行MkDocs容器
docker run --rm -it -p 8000:8000 -v $(pwd):/docs squidfunk/mkdocs-material

"# Mymkdocs" 
