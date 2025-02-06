using System;
using System.Diagnostics;

class PsExecManager
{
    static void RunCommand(string command)
    {
        ProcessStartInfo psi = new ProcessStartInfo
        {
            FileName = "psexec.exe",
            Arguments = command,
            RedirectStandardOutput = true,
            UseShellExecute = false,
            CreateNoWindow = true
        };

        Process process = new Process { StartInfo = psi };
        process.Start();
        Console.WriteLine(process.StandardOutput.ReadToEnd());
        process.WaitForExit();
    }

    static void Main()
    {
        string remotePC = "192.168.1.100";
        string processName = "notepad.exe";

        Console.WriteLine("プロセス一覧:");
        RunCommand($"\\\\{remotePC} tasklist");

        Console.Write("停止するプロセス名: ");
        string targetProcess = Console.ReadLine();
        RunCommand($"\\\\{remotePC} taskkill /IM {targetProcess} /F");

        Console.WriteLine($"{targetProcess} を停止しました。");

        Console.Write("再開するプロセス名: ");
        string startProcess = Console.ReadLine();
        RunCommand($"\\\\{remotePC} {startProcess}");

        Console.WriteLine($"{startProcess} を開始しました。");
    }
}

psexec.exe をダウンロードして C:\Windows\System32 に配置
RunCommand($"\\\\{remotePC} tasklist"); でリモートPCのプロセス取得
taskkill /IM notepad.exe /F でリモートPCのプロセスを強制終了
notepad.exe でプロセスを起動
