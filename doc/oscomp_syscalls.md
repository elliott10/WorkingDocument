# 系统调用的说明以及调用方式
系统调用方式遵循RISC-V ABI,即调用号存放在a7寄存器中,6个参数分别储存在a0-a5寄存器
中,返回值保存在a0中。

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

### #define SYS_fcntl 25
操作文件描述符；
```
int fd, int cmd;
int ret = syscall(SYS_fcntl, fd, cmd, (void *)arg);
```

### #define SYS_faccessat 48
检查一个文件的用户权限；
```
int fd, const char *filename, int amode;
int ret = syscall(SYS_faccessat, fd, filename, amode);
```

### #define SYS_chdir 49
切换工作目录；
```
const char *path;
int ret = syscall(SYS_chdir, path);
```

### #define SYS_waitid 95
等待进程改变状态；
```
idtype_t type, id_t id, siginfo_t *info, int options;
int ret = syscall(SYS_waitid, type, id, info, options, 0);

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

### #define SYS_fork 58
创建一个子进程；
```
pid_t ret = syscall(SYS_fork);
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

### #define SYS_getdents 61
获取目录的条目;
```
int fd, struct dirent *buf, size_t len
int ret = syscall(SYS_getdents, fd, buf, len);
```

### #define SYS_lseek 62
重新定位读写文件的偏移；
```
int fd, off_t offset, int whence;
off_t ret = syscall(SYS_lseek, fd, offset, whence);

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

### #define SYS_writev 66
写入数据到多个缓存区；
```
int fd, const struct iovec *iov, int count;
ssize_t ret = syscall(SYS_writev, fd, iov, count);
```

### #define SYS_pread 67
以给定的偏移量读取文件描述符;
```
int fd, void *buf, size_t size, off_t ofs;
ssize_t ret = syscall(SYS_pread, fd, buf, size, ofs);
```

### #define SYS_pwrite 68
以给定的偏移量写入到文件描述符
```
int fd, const void *buf, size_t size, off_t ofs;
ssize_t ret = syscall(SYS_pwrite, fd, buf, size, ofs);
```

### #define SYS_fstatat 79
获取文件状态；
```
int fd, const char *restrict path, int flag;
struct kstat kst;
int ret = syscall(SYS_fstatat, fd, path, &kst, flag);
```

### #define SYS_fstat 80
获取文件状态；
```
int fd;
struct kstat kst;
int ret = syscall(SYS_stat, fd, &kst);
```

### #define SYS_chmod 90
修改文件的mode位
```
const char *path, mode_t mode;
int ret = syscall(SYS_chmod, path, mode);
```

### #define SYS_chown 92
修改文件拥有者和组；
```
const char *path, uid_t uid, gid_t gid;
int ret = syscall(SYS_chown, path, uid, gid);
```

### #define SYS_exit 93
触发进程终止，无返回值；
```
int ec;
syscall(SYS_exit, ec);
```

### #define SYS_exit_group 94
退出一个进程的所有线程，无返回值；
```
int ec;
syscall(SYS_exit_group, ec);
```

### #define SYS_kill 129
发送一个信号给进程；
```
pid_t pid, int sig;
int ret = syscall(SYS_kill, pid, sig);
```

### #define SYS_rt_sigaction 134
检查并更改一个信号动作；
```
int sig;
struct k_sigaction ksa, ksa_old;
#define _NSIG     65
int ret = syscall(SYS_rt_sigaction, sig, &ksa, &ksa_old, _NSIG/8);

```

### #define SYS_times 153
获取进程时间；
```
struct tms *tms;
clock_t ret = syscall(SYS_times, tms);
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

### #define SYS_getpid 172
获取进程ID；
```
pid_t ret = syscall(SYS_getpid);
```

### #define SYS_getuid 174
获取用户ID；
```
uid_t ret = syscall(SYS_getuid);
```

### #define SYS_geteuid 175
获取用户ID；
```
uid_t ret = syscall(SYS_geteuid);
```

### #define SYS_getgid 176
获取组ID；
```
gid_t ret = syscall(SYS_getgid);
```

### #define SYS_getegid 177
获取组ID；
```
gid_t = syscall(SYS_getegid);

```

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

### #define SYS_mremap 216
重映射一段虚拟内存地址；
```
void *old_addr, size_t old_len, size_t new_len, int flags,void *new_addr;
void * ret = syscall(SYS_mremap, old_addr, old_len, new_len, flags, new_addr);

```

### #define SYS_mmap 222
将文件或设备映射到内存中；
```
void *start, size_t len, int prot, int flags, int fd, off_t off
long ret = syscall(SYS_mmap, start, len, prot, flags, fd, off);
```

### #define SYS_open 1024
打开或创建一个文件；
```
const char *filename, int flags, mode_t mode;
int ret = syscall(SYS_open, filename, flag, mode)
```

### #define SYS_link 1025
创建文件的链接；
```
const char *existing, const char *new;
int ret = syscall(SYS_link, existing, new);

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

### #define SYS_access 1033
检查一个文件的用户权限；
```
const char *filename, int amode;
int ret = syscall(SYS_access, filename, amode);

```

### #define SYS_stat 1038
获取文件状态；
```
const char *restrict path;
struct kstat kst;
int ret = syscall(SYS_stat, path, &kst);
```

### #define SYS_lstat 1039
获取文件状态；
```
const char *restrict path;
struct kstat kst;
int ret = syscall(SYS_lstat, path, &kst);
```

### #define SYS_time 1062
获取时间；
```
time_t *tloc;
time_t ret = syscall(SYS_time, tloc);
```
### #define SYS_getmainvars 2011
//


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



