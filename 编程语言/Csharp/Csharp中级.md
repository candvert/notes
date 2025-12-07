## Path类
```c#
using System.IO;

string folderPath = @"C:\MyDocuments";
string fileName = "report.pdf";
string fullPath = Path.Combine(folderPath, fileName);


string filePath = @"C:\Users\John\Documents\project\data.txt";
Path.GetFileName(filePath); // data.txt
Path.GetFileNameWithoutExtension(filePath); // data
Path.GetExtension(filePath); // .txt
Path.GetDirectoryName(filePath); // C:\Users\John\Documents\project
```
## File类
```c#
using System.IO;

string filePath = @"C:\example\test.txt";
if (File.Exists(filePath))
{
    Console.WriteLine("文件存在");
}

// 读取为字符串
string content = File.ReadAllText(filePath, Encoding.UTF8); // 可指定编码

// 按行读取
string[] lines = File.ReadAllLines(filePath, Encoding.Default);

// 读取为字节数组（如图片）
byte[] bytes = File.ReadAllBytes(filePath);

// 写入文本（覆盖）
File.WriteAllText(filePath, "Hello, World!", Encoding.UTF8);

// 按行写入（覆盖）
string[] newLines = { "Line 1", "Line 2", "Line 3" };
File.WriteAllLines(filePath, newLines, Encoding.UTF8);

// 追加文本
File.AppendAllText(filePath, "\nThis is appended text.", Encoding.UTF8);



string sourcePath = @"C:\source\file.txt";
string destPath = @"C:\destination\file.txt";

// 复制文件（第三个参数为true表示覆盖已存在的目标文件）
File.Copy(sourcePath, destPath, true);

// 移动文件（也可用于重命名）
File.Move(sourcePath, destPath); // 注意：跨磁盘移动可能有限制[9](@ref)

// 删除文件
File.Delete(destPath);

// 创建文件
FileStream fs = File.Create(@"C:\example\newfile.txt");
fs.Close();
```
## FileStream类
```c#
// FileStream 操作的是字节（byte）和字节数组
using System.IO;

string content = "Hello, FileStream!";
byte[] data = Encoding.UTF8.GetBytes(content); // 将字符串转换为字节数组

using (FileStream fs = new FileStream("example.txt", FileMode.Create, FileAccess.Write))
{
    fs.Write(data, 0, data.Length);
    fs.Flush();
}
```