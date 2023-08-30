# tinymce6 pastewordimage插件

**一款从word粘贴到富文本框图片正常显示插件，wps和office都可以正常复制粘贴图片**

<hr/>

### 使用步骤：

#### 第一步：把当前plugins目录下的pastewordimage文件夹拷贝到你的项目tinymce/plugins目录下去；
*例如react项目，路径为 项目根目录/public/tinymce/plugins/*

#### 第二步：tinymce初始化init配置plugins参属下增加pastewordimage；
*例如：*
```
// 如下增加pastewordimage参数

<Editor
    init={{
        branding: false, // 去掉右下角的水印
        menubar: true, // 菜单栏
        height: 500,
        plugins: [
            'advlist', 'autolink', 'lists', 'link', 'image', 'charmap', 'preview',
            'anchor', 'searchreplace', 'visualblocks', 'code', 'fullscreen',
            'insertdatetime', 'media', 'table', 'code', 'help', 'wordcount', 'codesample',
            'pastewordimage', // word复制图片插件
        ],
    }}
    initialValue="<p>This is the initial content of the editor.</p>"
    onInit={(evt, editor) => editorRef.current = editor}
    rootClassName={style.editor}
/>

```
#### 第三步：版当前项目的tinymce.min.js文件拷贝到项目的tinymce目录中进行覆盖；
*解释：*
```
// 源码中 wps粘贴单个图片时候粘贴的内容中会有text/html类型，导致getDataTransferItems函数返回结果不正确；
// 修改如下：items[contentType]赋值增加判断；

const getDataTransferItems = dataTransfer => {
    const items = {};
    if (dataTransfer && dataTransfer.types) {
        for (let i = 0; i < dataTransfer.types.length; i++) {
            const contentType = dataTransfer.types[i];
            try {
                // items[contentType] = dataTransfer.getData(contentType);
                items[contentType] = dataTransfer.types.indexOf('Files') === -1 ? dataTransfer.getData(contentType) : ''; // wps粘贴单个图片问题处理
            } catch (ex) {
                items[contentType] = '';
            }
        }
    }
    return items;
};

```
12311

