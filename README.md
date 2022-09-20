Typecho-Plugin-ExSearch
🔍 为 Typecho 带来实时搜索体验 build status

支持typecho 1.2.0

使用
下载本仓库（master 分支）：下载
解压文件夹，并将文件夹重命名为 ExSearch。
上传至插件目录，在后台启用
保存一次插件设置，并点击重建索引。
在主题中，在任何可点击的元素上加上 class="search-form-input"，点击即可唤起搜索框。

自定义 hook
默认的，点击搜索结果时会直接跳转至对应的页面，但是若你的主题使用了 AJAX 或者 PJAX 技术，你可能需要使用自定义的钩子来处理点击事件（例如发起一次 PJAX 操作）。在页面中插入一个函数如下：

<script>
function ExSearchCall(item){
    // your code
}
</script>
其中，item 是一个 JQuery 对象。举例：

function ExSearchCall(item){
    if (item && item.length) {
        $('.ins-close').click(); // 关闭搜索框
        let url = item.attr('data-url'); // 获取目标页面 URL
        $.pjax({url: url, 
            container: '#pjax-container',
            fragment: '#pjax-container',
            timeout: 8000, }); // 发起一次 PJAX 请求
    }
}
可能的问题
如果你的站点内容过多导致建立索引失败，请在 Plugin.php 第 136 行左右的位置，取消下面两行的注释：

$sql = 'SET GLOBAL max_allowed_packet=4294967295;';
$db->query($sql);
注意，这需要高级权限。你也可以手动对数据库执行：

mysql > SET GLOBAL max_allowed_packet=4294967295;
Credit
本项目灵感来源于 Wikitten 与 PPOffice，感谢。

This project is inspired by Wikitten and PPOffice, thanks.
