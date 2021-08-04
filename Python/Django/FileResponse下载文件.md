```py
import os
from django.http import FileResponse, Http404
def file_response_download(request):
    file_path = 'http://10.197.190.62:81/djangoUpload/Bseos/1618204543594a.mp4'
    file_path = '/static/1618204543594a.mp4'
    ext = os.path.basename(file_path).split('.')[-1].lower()
    # cannot be used to download py, db and sqlite3 files.
    if ext not in ['py', 'db',  'sqlite3']:
        from brms.settings import BASE_DIR
        response = FileResponse(open(BASE_DIR + file_path, 'rb'))
        response['content_type'] = "application/octet-stream"
        response['Content-Disposition'] = 'attachment; filename=' + os.path.basename(file_path)
        return response
    else:
        raise Http404
```

```js
function downVideo() {
      var fileUrl = 'http://10.197.190.62:81/djangoUpload/Bseos/1618204543594a.mp4'
      var videoUrl = '/djangoUpload/Bseos/1618204543594a.mp4'
      var xhr = new XMLHttpRequest();
      xhr.open("GET", '{% url 'bemsp:file_response_download' %}', true); // 也可以使用POST方式，根据接口
      xhr.responseType = "blob"; // 返回类型blob
      // 定义请求完成的处理函数，请求前也可以增加加载框/禁用下载按钮逻辑
      xhr.onload = function () {
        // 请求完成
        if (this.status === 200) {
          // 返回200
          var content = this.response;
          var elink = document.createElement('a');
          elink.download = 'page.js';
          elink.style.display = 'none';

          var blob = new Blob([content]);
          elink.href = URL.createObjectURL(blob);

          document.body.appendChild(elink);
          elink.click();

          document.body.removeChild(elink);
        }
      };
      // 发送ajax请求
      xhr.send();
    }
```

