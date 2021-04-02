# 系统调用的说明以及调用方式
系统调用方式遵循RISC-V ABI,即调用号存放在a7寄存器中,6个参数分别储存在a0-a5寄存器
中,返回值保存在a0中。

主要参考了Linux 5.10 syscalls
## 文件系统相关
### #define SYS_getcwd 17
获取当前工作目录；
```
char *buf, size_t size;
long ret = syscall(SYS_getcwd, buf, size);
```

### #define SYS_pipe 22 
创建管道；
```
int fd[2];
int ret = syscall(SYS_pipe, fd);
```

### #define SYS_dup 23
复制文件描述符；
```
int fd;
int ret = syscall(SYS_dup, fd);
```

### #define SYS_dup2 24
复制文件描述符，并指定了新的文件描述符；
```
int old, int new;
int ret = syscall(SYS_dup2, old, new);
```

### #define SYS_chdir 49
切换工作目录；
```
const char *path;
int ret = syscall(SYS_chdir, path);
```

### #define SYS_openat 56
打开或创建一个文件；
```
int fd, const char *filename, int flags, mode_t mode;
int ret = syscall(SYS_openat, fd, filename, flags, mode);
```

### #define SYS_close 57
关闭一个文件描述符；
```
int fd;
int ret = syscall(SYS_close, fd);
```

### #define SYS_getdents 61
获取目录的条目;
```
int fd, struct dirent *buf, size_t len
int ret = syscall(SYS_getdents, fd, buf, len);
```

### #define SYS_read 63
从一个文件描述符中读取；
```
int fd, void *buf, size_t count;
ssize_t ret = syscall(SYS_read, fd, buf, count);
```

### #define SYS_write 64
从一个文件描述符中写入；
```
int fd, const void *buf, size_t count;
ssize_t ret = syscall(SYS_write, fd, buf, count);
```

### #define SYS_open 1024
打开或创建一个文件；
```
const char *filename, int flags, mode_t mode;
int ret = syscall(SYS_open, filename, flag, mode)
```

### #define SYS_unlink 1026
移除指定文件的链接；
```
const char *path;
int ret = syscall(SYS_unlink, path);
```

### #define SYS_mkdir 1030
创建目录；
```
const char *path, mode_t mode;
int ret = syscall(SYS_mkdir, path, mode);
```

### #define SYS_umount2 39
卸载文件系统；
```
const char *special, int flags;
int ret = syscall(SYS_umount2, special, flags);
```

### #define SYS_mount 40
挂载文件系统；
```
const char *special, const char *dir, const char *fstype, unsigned long flags, const void *data;
int ret = syscall(SYS_mount, special, dir, fstype, flags, data);
```

### #define SYS_fstat 80

获取文件状态；
```
int fd;
struct kstat kst;
int ret = syscall(SYS_stat, fd, &kst);
```

## 进程管理相关

### #define SYS_waitid 95
等待进程改变状态；
```
idtype_t type, id_t id, siginfo_t *info, int options;
int ret = syscall(SYS_waitid, type, id, info, options, 0);

```

### #define SYS_clone 220
创建一个子进程；
```
pid_t ret = syscall(SYS_clone, flags, stack, ptid, tls, ctid)
```

### #define SYS_execve 59
执行程序；
```
const char *path, char *const argv[], char *const envp[];
int ret = syscall(SYS_execve, path, argv, envp);
```
### #define SYS_wait 55
等待进程改变状态;
```
pid_t wait(int *wstatus);
```

### #define SYS_waitpid 60
等待进程改变状态;
```
pid_t waitpid(pid_t pid, int *wstatus, int options);
```

### #define SYS_exit 93
触发进程终止，无返回值；
```
int ec;
syscall(SYS_exit, ec);
```

### #define SYS_getppid 173
获取父进程ID；
```
pid_t ret = syscall(SYS_getppid);
```


### #define SYS_getpid 172
获取进程ID；
```
pid_t ret = syscall(SYS_getpid);
```


## 内存管理相关
### #define SYS_sbrk 213
修改数据段的大小；


### #define SYS_brk 214
修改数据段的大小；
```
uintptr_t brk;
uintptr_t ret = syscall(SYS_brk, brk);
```

### #define SYS_munmap 215
将文件或设备取消映射到内存中；
```
void *start, size_t len
int ret = syscall(SYS_munmap, start, len);
```

### #define SYS_mmap 222
将文件或设备映射到内存中；
```
void *start, size_t len, int prot, int flags, int fd, off_t off
long ret = syscall(SYS_mmap, start, len, prot, flags, fd, off);
```

## 其他

### #define SYS_time 1062

获取时间；
```
time_t *tloc;
time_t ret = syscall(SYS_time, tloc);
```

### #define SYS_uname 160
打印系统信息；
```
struct utsname *uts;
int ret = syscall(SYS_uname, uts);
```

### #define SYS_gettimeofday 169
获取时间；
```
struct timespec *ts;
int ret = syscall(SYS_gettimeofday, ts, 0);
```

## 调用

```
static inline _u64 internal_syscall(long n, _u64 _a0, _u64 _a1, _u64 _a2, _u64
		_a3, _u64 _a4, _u64 _a5) {
	register _u64 a0 asm("a0") = _a0;
	register _u64 a1 asm("a1") = _a1;
	register _u64 a2 asm("a2") = _a2;
	register _u64 a3 asm("a3") = _a3;
	register _u64 a4 asm("a4") = _a4;
	register _u64 a5 asm("a5") = _a5;
	register long syscall_id asm("a7") = n;
	asm volatile ("ecall" : "+r"(a0) : "r"(a1), "r"(a2), "r"(a3), "r"(a4), "r"
			(a5), "r"(syscall_id));
	return a0;
}
```
