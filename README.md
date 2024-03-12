# minishell
minishell

***

### 명령어 정리

* readline
  
  ```c
  char *readline(const char *);
  ```
  함수 설명
  >  입력받은 문자열을 저장하고 그 메모리주소를 반환한다.

* rl_clear_history
  
  ```c
  #include <readline/readline.h>
  void rl_clear_history(void);
  ```
  함수 설명
  >  현재 history를 지운다.
  반환값
  >  없다.  
  
* rl_on_new_line

  ```c
  #include <readline/readline.h>
  int rl_on_new_line(void);
  ```
  함수 설명  
  >  readline 디렉토리 내에서 함수들에게 커서가 개행 문자를 통해 다음 줄로 이동했음을 알려줄 때  
    이용되는 함수 알림 용도의 함수이므로 직접적으로 rl_on_new_line 함수가 개행 문자를 수행해주지는 않는다.  
    
  반환값  
  >  성공 → 0 실패 → -1  
  
* rl_replace_line
  ```c
  #include <readline/readline.h>
  void rl_replace_line(const char *text, int clear_undo);
  ```
  함수 설명  
  >  rl_line_buffer라는 변수를 사용하는데 rl_line_buffer는 사용자가 입력한 문자열을 별도로 유지한다.  
    rl_line_buffer에 입력받은 내용을 text라는 문자열로 대치한다.
  >   
  > clear_undo는 내부적으로 유지 중인 undo_list를 초기화할 지의 여부를 결정 짓는 값이다.  
    clear_undo == 0 → 초기화하지 않고, clear_undo == 1 → 초기화한다.
    
  반환값  
  >  반환값이 없다  
  
* rl_redisplay
  ```c
  #include <readline/readline.h>
  void rl_redisplay(void);
  ```
  함수 설명  
  >  rl_line_buffer라는 변수를 이용하는데 사용자가 입력하여 유지 중인 rl_line_buffer의 값을 프롬프트와 함께 출력해준다.  
    이 때 프롬프트 값을 readline 함수에 prompt로 준 문자열로 이용된다.
      
  반환값  
  >  없다.  
    
* add_history
  ```c
  #include <readline/readline.h>
  int add_history(const char *line);
  void add_history(const char *line);
  ```
  함수 설명
  >  readline 함수의 기본 동작 중에 사용자가 입력했던 문자열을 다시 얻게 해주는 함수이다.  
    add_history의 인자인 line으로 기재한 문자열은 위와 아래 방향키를 통해서 readline 함수 실행 도중에 다시 불러올 수 있다.
    
  반환값
  >  Unix 계열에서 내장된 readline 디렉토리를 이용하는 경우에는 int 타입으로 반환값을 만드는데, 함수 수행에 문제가 없다면 → 0, 그렇지 않다면 → -1을 반환한다.  
    만약에 Unixt 계열에 내장된 readline 디렉토리가 아니라 GNU Library의 readline을 이용한다면, 이전과 달리 void 타입의 반환값을 만드는 것을 볼 수 있다.  
    사용하고 있는 readline의 디렉토리가 어떤 것인지 확인하는 게 좋다.  
  [사용 예시](https://wtg-study.tistory.com/103)

* access
  
* open
  
* read
  
* close
  
* fork
  
* wait
  
* waitpid
  
* wait3, wait4
  ```c
  #include <sys/wait.h>
  #include <sys/resource.h>
  pid_t wait3(int *stat_loc, int options, struct rusage *rusage);
  pid_t wait4(pid_t pid, int *stat_loc, int options, struct rusage *rusage);
  ```
  함수 설명  
  >  wait for process termination 자식 프로세스가 종료되는 것을 기다리며, 종료된 프로세스의 상태와 자원 사용량을 알려주는 함수  
    wait3 statloc : 자식 프로세스의 exit code를 가지고 있다.  
    options : 자식 프로세스를 어떻게 기다릴 건지 옵션(waitpid랑 똑같다)  
    rusage : 자식 프로세스의 리소스 사용량에 대한 정보가 담긴다.  
  >>wait4 : wait3이랑 똑같다. pid : waitpid의 인자와 똑같다.  
    
  반환값  
   > 성공 → 죽은 자식 프로세스의 pid, 실패 → -1  
  
* signal
  ```c
  #include <signal.h>
  void (*signal(int sig, void (*func)(int)))(int);

  //struct가 있으면 ->
  typedef void (*sig_t) (int); //반환값이 void형이고, 매개변수가 int형인 함수 포인터의 별명을 sig_t
  sig_t signal(int sig, sig_t func);
  ```
  함수 설명 simplified software signal facilities  
  >  코어 덤프 : 프로그램이 비정상적으로 종료(segfault..)할 때 프로그램에서 작업하던 메모리 상태를 저장하는 파일을(코어 파일) 만들고 종료한다.  
    sig : 처리할 시그널 번호 kill함수로 시그널을 전송할 때도 일어날 수 있다.  
  
  |번호|시그널|기본처리|발생조건|
  |:--|:--|:--|:--|
  |1 |  SIGHUP	|종료	|HangUP, 터미널에서 접속이 끊겼을 때 보내진다.|
  |2 |  SIGINT	|종료	|키보드로 Ctrl + c|
  |9 |	SIGKILL	|종료	|강제 종료시|
  |11|	SIGSEGV	|코어 덤프	|segfault가 생겼을 때|
  |12|	SIGSYS	|코어 덤프	|system call을 잘못 했을 때|
  |16|	SIGUSR1	|종료	|사용자 정의 시그널1|
  |17|	SIGUSR2	|종료	|사용자 정의 시그널2|
  |23|	SIGSTOP	|중지	|이 시그널을 받으면 SIGCONT 시그널을 받을 때까지 프로세스를 중지|
  |24|	SIGTSTP	|중지	|키보드로 Ctrl + z|
  |25|	SIGCONT	|무시	|중지된 프로세스|
  
* sigaction
  
* sigemptyset
  
* sigaddset
  
* kill
  
* exit
  
* getcwd
  
* chdir
  
* stat, lstat, fstat
  ```c
  #include <sys/stat.h>
  #include <unistd.h>
  int fstat(int fildes, struct stat *buf);
  int lstat(const char *restrict path, struct stat *restrict buf);
  int stat(const char *restrict path, struct stat *restrict buf);
  //struct status : 파일 정보를 저장하는 구조체
  ```
    
  함수 설명  get file status  
  >fstat fildes : 연 파일의 파일 디스크립터  
  buf : fildes가 가리키는 파일의 상태 및 정보를 저장할 구조체  
  >    
  >lstat, stat path : stat을 얻고자 하는 파일의 경로   
  buf : path에 있는 파일의 상태 및 정보를 저장할 구조체  
  >>차이점 : stat은 지정한 파일이 심볼릭 링크면 링크를 따라가서 원본 파일의 정보를 전달하지만,    
           lstat은 지정한 파일이 심볼릭 링크면 링크 파일 자체의 정보를 전달.  
  
  반환값  
  >stat, lstat, fstat 성공 → 0, 실패 → -1 그리고 errno가 설정된다.  

* unlink
  
* execve
  
* dup
  
* dup2
  
* pipe
  
* opendir
  
* readdir
  
* closedir
  
* strerror
  
* perror
  
* isatty
  
* ttyname
  
* ttyslot
  
* ioctl
  
* getenv
  
* tcsetattr
  
* tcgetattr
  
* tgetent
  
* tgetflag
  
* tgetnum
  
* tgetstr
  
* tgoto
  
* tputs
  
