---
title: Jenkins Email Extension插件模板
copyright: true
date: 2018-05-06 09:52:30
tags:
  - jenkins
  - Jenkins Email Extension
categories:
---

# Jenkins Email Extension插件模板

```
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <title>${ENV, var="JOB_NAME"}-第${BUILD_NUMBER}次构建日志</title>
</head>

<body leftmargin="8" marginwidth="0" topmargin="8" marginheight="4" offset="0">
    <table width="95%" cellpadding="0" cellspacing="0" style="font-size: 11pt; font-family: Tahoma, Arial, Helvetica, sans-serif">
        <tr>
            <td>(本邮件由程序自动下发，请勿回复！)</td>
        </tr>
        <tr>
            <td>
                <h2><font color="#FF0000">构建结果 - ${BUILD_STATUS}</font></h2>
            </td>
        </tr>
        <tr>
            <td>
                <br />
                <b><font color="#0B610B">构建信息</font></b>
                <hr size="2" width="100%" align="center" />
            </td>
        </tr>
        <tr> SVN_URL
            <td>
                <ul>
                    <li>项目名称：${PROJECT_NAME}</li>
                    <li>GIT路径：${GIT_URL}</li>
                    <li>构建编号：${BUILD_NUMBER}</li>
                    <li>GIT分支：${GIT_BRANCH}</li>
                    <li>触发原因：${CAUSE}</li>
                    <li>构建日志：<a href="${BUILD_URL}console">${BUILD_URL}console</a></li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <b><font color="#0B610B">变更信息:</font></b>
                <hr size="2" width="100%" align="center" />
            </td>
        </tr>
        <tr>
            <td>
                <ul>
                    <li>上次构建成功后变化 : ${CHANGES_SINCE_LAST_SUCCESS}</a>
                    </li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <ul>
                    <li>上次构建不稳定后变化 : ${CHANGES_SINCE_LAST_UNSTABLE}</a>
                    </li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <ul>
                    <li>历史变更记录 : <a href="${PROJECT_URL}changes">${PROJECT_URL}changes</a></li>
                </ul>
            </td>
        </tr>
        <tr>
            <td>
                <ul>
                    <li>变更集:${JELLY_SCRIPT,template="html"}</a>
                    </li>
                </ul>
            </td>
        </tr>
        <!--
        <tr>
            <td>
                <b><font color="#0B610B">Failed Test Results</font></b>
                <hr size="2" width="100%" align="center" />
            </td>
        </tr>
        <tr>
            <td>
                <pre style="font-size: 11pt; font-family: Tahoma, Arial, Helvetica, sans-serif">$FAILED_TESTS</pre>
                <br />
            </td>
        </tr>
        
        <tr>
            <td>
                <b><font color="#0B610B">构建日志 (最后 100行):</font></b>
                <hr size="2" width="100%" align="center" />
            </td>
        </tr>-->
        <!-- <tr>
            <td>Test Logs (if test has ran): <a
                href="${PROJECT_URL}ws/TestResult/archive_logs/Log-Build-${BUILD_NUMBER}.zip">${PROJECT_URL}/ws/TestResult/archive_logs/Log-Build-${BUILD_NUMBER}.zip</a>
                <br />
            <br />
            </td>
        </tr> -->
        <!--
        <tr>
            <td>
                <textarea cols="80" rows="30" readonly="readonly" style="font-family: Courier New">${BUILD_LOG, maxLines=100,escapeHtml=true}</textarea>
            </td>
        </tr>-->
        <hr size="2" width="100%" align="center" />
    </table>
</body>

</html>
```
