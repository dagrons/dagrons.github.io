<!--
.. title: python执行外挂-subprocess
.. slug: pythonzhi-xing-wai-gua-subprocess
.. date: 2021-09-19 13:28:14 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

# python执行外挂-subprocess

相比于os.system, subprocess允许我们直接创建进程, 而非从shell再创建任务, 而且允许我们对创建的进程执行更多的动作

## Import Concepts 
- A subprocess in Python is a task that a python scripts delegates to the Operating System (OS), That invokes wokring with the standard input *stdin*, standard output *stdout*, and return codes

- 进程和程序是存在区别的, 进程是程序的容器, 进程的创建过程如下: 发出进程创建请求, 操作系统创建进程容器, 操作系统为进程配置好输入输出, pid, 环境变量等容器环境, 然后操作系统加载程序到容器中, 进行执行, 在Unix，，这分别是由fork()和exec()完成的

## Popen Object
```python
Popen(args, bufsize=0, executable=None, stdin=None, stdout=None, stderr=None, preexec_fn=None, close_fds=False, shell=False, cwd=None, env=None, universal_newlines=False, startupinfo=None, creationflags=0)

args: a string 类似bash commands, 其中args[0]为command name

executable: executable program, 和args[0]区别在于command name是ps等命令会显示的display name, 而executble是真正的可执行程序

close_fds: if true, all file descriptors except 0, 1 and 2 will be closed before the child process is executed. 即子进程不会继承当前进程的file descriptors(除了0, 1, 2)

shell=True: 等价于Popen(['/bin/sh', '-c', args[0], args[1], ...])

cwd: current working directory

env: a mapping defines the environment variables for the new process

```

## Popen Instance Methods & Attributes
```
Popen.poll() # if a child process has terminated
Popen.wait() # wait a child process to terminated 
Popen.communicate(input=None) # send data to stdin, return (stdoutdata, stderrdata) 
Popen.send_signal(signal) # send signal to child process
Popen.terminate()
Popen.kill()
Popen.stdin
Popen.stdout
Popen.stderr
Popen.pid
Popen.returncode # None if hasn't terminated yet. -N indicate that the child was terminated by signal N(Unix only)
```


## Popen简化版

**call**
Run command with arguments. Wait for command to complete, then return the retcode

```python 
retcode = subprocess.call(["ls", "-l"])
```

**check_output**
Run command with arguments and return its output as a byte string 

```python
subprocess.check_output(["ls", "-l", "/dev/null"]) #=> 'crw-rw-rw- 1 root root ... '
```

## Exceptions

**OSError** errors when trying to execute a non-existent file. Application should prepare for OSError exception

## Cheatsheet

```
output=`mycmd myarg`
==>
output = Popen(["mycmd", "myarg"], stdout=PIPE).communicate()[0]
```

```
output=`dmesg | grep hda`
==>
p1 = Popen(["dmesg"], stdout=PIPE)
p2 = Popen(["grep", "hda"], stdin=p1.stdout, stdout=PIPE)
p1.stdout.close()  # Allow p1 to receive a SIGPIPE if p2 exits.
output = p2.communicate()[0]
```

```
sts = os.system("mycmd" + " myarg")
==>
p = Popen("mycmd" + " myarg", shell=True)
sts = os.waitpid(p.pid, 0)[1]
```

```
pid = os.spawnlp(os.P_NOWAIT, "/bin/mycmd", "mycmd", "myarg")
==>
pid = Popen(["/bin/mycmd", "myarg"]).pid
```

```
retcode = os.spawnlp(os.P_WAIT, "/bin/mycmd", "mycmd", "myarg")
==>
retcode = call(["/bin/mycmd", "myarg"])
```

```
os.spawnlpe(os.P_NOWAIT, "/bin/mycmd", "mycmd", "myarg", env)
==>
Popen(["/bin/mycmd", "myarg"], env={"PATH": "/usr/bin"})
```