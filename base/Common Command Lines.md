- "下载单独一个文件"
- `$url = "https://github.com/WSLeon/WSLeon/raw/main/static/touxiang.jpg"`
- `$fileExtension = [System.IO.Path]::GetExtension($url)`
- `$fileName = [System.IO.Path]::GetFileName($url)`
- `Invoke-WebRequest -Uri $url -OutFile $fileName`
- `Invoke-WebRequest -Uri https://github.com/WSLeon/WSLeon/raw/main/static/touxiang.jpg -OutFile touxiang.jpg`
- powershell

# GitHub 仓库信息
$repoUser = "用户名"
$repoName = "仓库名"
$branch = "分支名"
$folderPath = "文件夹路径"

# 获取文件夹内容列表
$apiUrl = "https://api.github.com/repos/$repoUser/$repoName/contents/$folderPath?ref=$branch"
$response = Invoke-WebRequest -Uri $apiUrl -Headers @{ "Authorization" = "token YourAccessToken" }
- $apiUrl后面内容可以删除
# 将响应内容转换为 JSON 格式
$folderContent = $response.Content | ConvertFrom-Json

# 指定文件夹保存路径
$downloadFolderPath = "D:\下载目录\$folderPath"
mkdir -Force $downloadFolderPath

# 遍历文件夹内容并下载文件
foreach ($file in $folderContent) {
if ($file.type -eq "file") {
$downloadUrl = $file.download_url
$fileName = [System.IO.Path]::GetFileName($file.name)
$filePath = Join-Path -Path $downloadFolderPath -ChildPath $fileName
Invoke-WebRequest -Uri $downloadUrl -OutFile $filePath
}
}

Write-Host "文件夹已下载完成。"