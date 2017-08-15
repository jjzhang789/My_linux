##Nginx+Lua安装部署



nginx lua模块淘宝开发的nginx第三方模块，它能将lua语言嵌入到nginx配置中，从而使用lua就极大增强了nginx的能力。nginx以高并发而知名，lua脚本轻便，两者的搭配堪称完美。接下来请看如何安装nginx + ngx_lua模块，以及最后来个简单的测试。

####一、依赖安装
为了解决烦人的报错，故先安装好依赖。
>yum install -y gcc-c++ openssl-devel pcre-devel gcc

####二、LuaJit安装
<pre>
下载：
wget http://luajit.org/download/LuaJIT-2.0.5.tar.gz

解压并安装：
tar -xvf LuaJIT-2.0.5.tar.gz && cd LuaJIT-2.0.5
make && make install

配置环境变量：
echo "export LUAJIT_LIB=/usr/local/lib" >>/etc/profile
echo "export LUAJIT_INC=/usr/local/include/luajit-2.0" >>/etc/profile
cp /usr/local/include/luajit-2.0/* /usr/local/include/
ln -s /usr/local/lib/libluajit-5.1.so.2 /usr/lib64/libluajit-5.1.so.2
source /etc/profile
</pre>

####三、Lua-cjson安装
<pre>
下载:
wget https://www.kyne.com.au/~mark/software/download/lua-cjson-2.1.0.tar.gz

解压并安装：
tar -xvf lua-cjson-2.1.0.tar.gz
cd lua-cjson-2.1.0
make && make install
</pre>

####四、下载所需模块
<pre>
下载模块：
wget https://github.com/openresty/echo-nginx-module/archive/v0.60.tar.gz -O echo-nginx-module_v6.0.tar.gz

wget https://github.com/openresty/lua-nginx-module/archive/v0.10.10.tar.gz -O lua-nginx-module_v0.10.10.tar.gz

wget https://github.com/simpl/ngx_devel_kit/archive/v0.3.0.tar.gz -O ngx_devel_kit.v0.3.0.tar.gz

解压模块：
tar -xvf echo-nginx-module_v6.0.tar.gz
tar -xvf lua-nginx-module_v0.10.10.tar.gz
tar -xvf ngx_devel_kit.v0.3.0.tar.gz
</pre>

####五、Nginx安装
<pre>
下载nginx:
wget http://nginx.org/download/nginx-1.10.3.tar.gz

解压并安装：
tar -xvf nginx-1.10.3.tar.gz && cd nginx-1.10.3

./configure --prefix=/usr/local/nginx \
--sbin-path=/usr/bin/nginx \
--conf-path=/etc/nginx/nginx.conf \
--with-pcre --with-http_stub_status_module \
--with-http_ssl_module --with-http_realip_module \
--with-http_addition_module --with-http_sub_module --with-http_dav_module \
--with-http_flv_module --with-http_gzip_static_module \
--with-http_random_index_module --with-http_secure_link_module \
--with-http_degradation_module --with-http_stub_status_module --with-file-aio \
--with-poll_module --with-select_module \
--add-module=../echo-nginx-module-0.60 \
--add-module=../lua-nginx-module-0.10.10 \
--add-module=../ngx_devel_kit-0.3.0

make && make install
</pre>

####六、验证
做事要有始有终，不能安装好就完事了，还要验证一下。
<pre>
在nginx.conf中添加如下配置：
 location  /luatest {
      	default_type 'text/plain';
      	content_by_lua 'ngx.say("Hello, Lua")';
        }
</pre>

启动Nginx后在浏览器输入：http://ip/luatest 页面出现"Hello, Lua"表示安装成功。