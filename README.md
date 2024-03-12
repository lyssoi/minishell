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
  
  >  func : 시그널을 처리할 핸들러, sig가 들어왔을 때 작동할 함수, 매개변수는 sig다.  
  >         만약에 func이 NULL 포인터라면, sig의 기본 처리를 실행한다.  
    
  반환값  
  >  성공 → 이전에 설정된 시그널 핸들러, 만약에 전에 설정된 핸들러가 없다면 0  
  >  실패 → SIG_ERR  
  
  
* sigaction
  ```c
  #include <signal.h>
  
  struct sigaction
  {
  	void (*sa_handler)(int); //시그널을 처리하기 위한 핸들러
  													 //SIG_DFS, SIG_IGN 또는 핸들러 함수
  	void (*sa_sigaction)(int, siginfo_t *, void *); //밑의 sa_flags가 SA_SIGINFO일 때
  																									//sa_handler 대신에 동작하는 핸들러
  	sigset_t sa_mask; //시그널을 처리하는 동안 블록화할 시그널 집합의 마스크
  	int      sa_flags; //아래 설명 참고
  	void (*sa_restorer)(void); //사용해서는 안 된다.
  }
  
  int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact);
  ```
  
  함수 설명
  >  sig : 시그널 번호
  >  act : 설정할 행동. 즉, 새롭게 지정할 처리 행동
  >  oldact : 이전 행동, 이 함수를 호출하기 전에 지정된 행동 정보가 입력된다. 포인터로 보냈기 때문에 값이 지정된다.
  >  - signal()에서는 처리할 행동 정보롤 시그널이 발생하면 호출이될 함수 포인터를 넘겨 주었는데,
  >    sigaction()에서는 struct sigaction 구조체 값을 사용해서 좀 더 다양한 지정이 가능하다(예 : 받지 않을 시그널)
  반환값
  >  성공 → 0, 실패 → -1  
  
* sigemptyset
  ```C
  #include <signal.h>
  int sigemptyset(sigset_t *set);
  ```
  함수 설명  
  >  인자로 온 set을 빈 시그널 집합으로 만든다.
    
  반환값  
  >  성공 → 0, 실패 → -1  
* sigaddset
  ```c
  #include <signal.h>
  int sigaddset(sigset_t *set, int signum);
  ```
  함수 설명  
  >  시그널 집합 set에 signum을 추가한다.    
      
  반환값  
  >  성공 → 0, 실패 → -1  
  
* kill

  ```c
  #include <signal.h>
  int raise(int sig);
  ```
  
  함수 설명  
  >  kill() 함수는 쉘에서 프로세스를 죽이는 kill 명령과는 달리 프로세스에 시그널을 전송한다.  
  >  물론, 프로세스에 SIGKILL을 보내면 쉘 명령의 kill과 같은 역할을 한다.    
  >  인수: int sig 전송하려는 시그널 번호  
     
  반환값  
  >  성공 -> 0  
  >  실패 -> !0  
  
* getcwd
  ```c
  #include <unistd.h>
  char *getcwd(char *buf, size_t size);
  ```
    
  함수 설명 get working directory pathname  
  >  현재 경로를 절대 경로로 buf에다가 넣어준다.  
  >  size는 buf에 넣을 수 있는 문자 수.  
  >  buf에 NULL 포인터가 들어가면 현재 절대경로를 malloc한 후 복사하고 리턴을 해준다.  
    
  반환값
  >  성공 → buf, buf자리에 NULL일 때 → 동적할당하고 복사한 걸 리턴  
  >  실패 → NULL 포인터  

* chdir
  ```c
  #include <unistd.h>
  int chdir(const char *path);
  ```
  
  함수 설명 change current working directory  
  >  cd할 때 사용? path로 현재 디렉토리를 바꾼다.(path로 경로 변경)  
    
  반환값  
  >  성공 → 0, 실패 → -1 그리고 errno 설정
  
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
  
* opendir, readdir, closedir
  ```c
  #include <dirent.h>
  DIR *opendir(const char *filename);
  struct dirent *readdir(DIR *dirp);
  int closedir(DIR *dirp);
  ```
  함수 설명  
  >  directory operations fopen에서 파일 포인터(FILE *)를 받아서 fread에 사용하고  
  >  fclose로 닫는 것처럼 opendir로 DIR *를 받아서 readdir로 읽고 closedir로 닫는다.  
  >  opendir filename : 열 디렉토리의 이름 연 후에 DIR *를 반환한다.   
  >  readdir dirp : 정보를 가져올 디렉토리의 디렉토리 포인터(DIR *) 디렉토리에 있는 내용을 읽고 다음 읽을 디렉토리로 DIR *를 옮긴다.  
  >  그리고 끝까지 다 읽었을 때는 NULL를 리턴한다.  
  >  closedir dirp : 닫을 디렉토리 스트림  
    
  반환값  
  >  opendir 성공 → 연 디렉토리 포인터, 실패 → NULL  
  >  readdir 성공 → 디렉토리의 정보, 끝까지 다 읽었을 때 → NULL, 실패 → NULL, 그리고 errno 설정 끝까지 다 읽었을 때와 실패일 때의 반환값이 같기 때문에 실패인지를 판별할 때는 errno변수의 변경 유무로 확인한다.  
  >  closedir 성공 → 0, 실패 → -1  
  
* strerror
  
* perror
  
* isatty, ttyname
  ```c
  #include <unistd.h>
  int isatty(int fd);
  char *ttyname(int fd);
  ```
    
  함수 설명 get name of associated terminal (tty) from file descriptor  
  >  isatty 매개변수로 온 file descriptor가 터미널 장치와 관련됐는지 알려 준다.  
  >  ttyname fd가 속해 있는 터미널 장치의 이름을 가져온다.  
  >  ttyname이 반환한 문자열은 바뀔 수도 있다. 이유는 static 자료형을 가리킬 수도 있기 때문.  
    
  반환값    
  >  isatty fd가 터미널 장치와 관련됐다. → 1, 아니다 → 0  
  >  ttyname 성공 → 터미널 장치의 경로, 실패 → NULL 포인터
  
* ttyslot
  ```c
  #include <unistd.h>
  int ttyslot(void);
  ```
    
  함수 설명  
  >  find the slot of the current user’s terminal in some file ttyslot를 부른 프로그램이 참조하고 있는 터미널의 인덱스를 반환한다.  
     
  반환값  
  >  성공 → 터미널의 인덱스, 실패 → -1, 0  
* ioctl
  ```c
  #include <sys/ioctl.h>
  int ioctl(int fd, int cmd, ...);
  ```
  
  함수 설명
  >  ioctl은 "입출력 제어(I/O control)"의 줄임말로, read(), write() 이외의 장치에 특화된 입출력, 제어 동작을 수행하고자 할 때 사용되는 시스템 호출이다.  
  >  하드웨어에 데이터를 쓰거나 읽어올 때, 일반적인 R/W 함수의 동작만으로는 하드웨어의 동작 상태에 따라 처리되지 못하는 데이터가 종종 발생하기 때문에 리눅스 커널에서는 ioctl을 제공한다.
  >  ioctl 함수는 read나 write 작업도 처리할 수 있지만, 하드웨어의 제어나 상태를 얻기 위해 주로 사용되는 함수이다.
  > cmd 인수로 수행할 동작 지정  
  >  추가적인 인수 사용 가능  
  
  >  >  request에 명령어를 구성할 때, 이렇게 4개의 매크로를 제공해주는데  
  >  > 1. _IO(type, nr) : 이 매크로의 경우에는 세번째 인자를 사용안해도 될 때 해당 매크로를 사용한다.  
  >  >  
  >  > 2. _IOR(type, nr, size) : iotcl()의 세번째 인자가 디바이스 드라이버에서 read가 일어날 떄 사용한다.  
  >  > (kernel -> user 메모리 영역으로 복사가 일어나는 copy_to_user 동작이 발생할 때)  
  >  >    
  >  > 3. _IOW(type, nr, size) : iotcl()의 세번째 인자가 디바이스 드라이버에서 write가 일어날 때 사용한다.  
  >  > (kernel <- user 메모리 영역으로 쓰기가 일어나는 copy_from_user 동작이 발생할 때)  
  >  >    
  >  > 4. _IORW(type, nr, size) : ioctl() 함수의 세 번째 인수가 read, write 동작에 모두 사용될 때 사용한다.  
  >  >    
  >  >  > type : 여러 디바이스 드라이버 사이에서 ioctl() 함수의 명령어가 서로 유일한 값을 가질 수 있도록 사용하는 8비트 매직넘버.  
  >  >  > nr : 명령어를 구분하기 위해 사용되는 명령어마다 유일한 정수값.  
  >  >  > size : 디바이스 드라이버로 데이터가 넘어오거나 사용자 영역으로 넘겨 줄 때 사용하는 데이터 크기를 바이트 단위로 나타낸다.  
    
  반환값  
  >  일반적으로 성공 시 0을 반환한다. 몇몇 ioctl() 요청은 반환 값을 출력 매개변수로 사용하여 성공 시 음수 아닌 값을 반환한다.  
  >  오류 시 -1을 반환하며 오류를 나타내도록 errno를 설정한다.    

* getenv
  ```c
  #include <stdlib.h>
  char *getenv(const char *name);
  ```

  함수 설명 environment variable function  
  >  환경변수 리스트에서 name인 변수를 찾는다. 환경변수 리스트는 name=value의 형태. =을 넣어도 찾는다?
    
  반환값  
  > 성공 → 찾은 value, 실패 → NULL 포인터  
    
* tcsetattr, tcgetattr
  ```c
  #include <termios.h>
  int tcsetattr(int fildes, int optional_actions,
  							const struct termios *termios_p);
  int tcgetattr(int fildes, struct termios *termios_p);
  ```
  함수 설명 manipulating the termios structure
  >  tcsetattr fildes : 터미널의 file descriptor(터미널의 fd가 아닌 경우. 즉, 일반 파일의 경우는 오류다, STDIN 등은 가능하다.)  
  >  termios_p : fildes가 가리키는 터미널의 속성을 저장할 변수의 주솟값  
  >  tcgetattr fildes : 터미널 file descriptor optional_actions : 동작 선택  
    
  >  >TCSNOW : 속성(termios_p)를 바로 변경한다.  
  >  >TCSADRAIN : 현재 출력이 완료됐을 때 값을 변경한다.(fd로 써진 출력값이 터미널로 다 보내진 후에 변경된다.)  
  >  >This value of optional_actions should be used when changing parameters that affect output  
  >  >TSCAFLUSH : 현재 출력이 완료됐을 때 값을 변경한다. 하지만 현재 읽을 수 있으며, read 호출에서 아직 반환되지 않은 입력값은 버린다.  
    
  >  termios_p : 터미널 속성을 설정할 포인터  

  반환값  
  >  둘 다, 성공 → 0, 실패 → -1 그리고 errno 설정  
    
* tgetent, tgetflag, tgetnum, tgetstr, tgoto, tputs
  ```c
  #include <curses.h>
  #include <term.h>
  
  extern char PC;
  extern char * UP;
  extern char * BC;
  extern unsigned ospeed;
  
  int tgetent(char *bp, const char *name);
  int tgetflag(char *id);
  int tgetnum(char *id);
  char *tgetstr(char *id, char **area);
  char *tgoto(const char *cap, int col, int row);
  int tputs(const char *str, int affcnt, int (*putc)(int));
  ```
  이러한 루틴은 termcap 라이브러리를 사용하는 프로그램들을 변환하기 위한 도우미로 포함되어 있다. 이들의 매개변수는 동일하며 루틴은 terminfo 데이터베이스를 사용하여 에뮬레이션된다.
  따라서 이들은 컴파일된 terminfo 항목에 대한 능력을 조회하는 데만 사용할 수 있다.

  * >tgetent 루틴은 name에 대한 항목을 로드한다. 성공하면 1을 반환하고, 해당 항목이 없으면 0을 반환하며, terminfo 데이터베이스를 찾을 수 없으면 -1을 반환한다.  
      이러한 에뮬레이션에서는 버퍼 포인터 bp를 무시한다.  
  * >tgetflag 루틴은 id에 대한 부울 항목을 가져오거나 해당 항목이 사용 불가능한 경우 0을 반환한다.  
  * >tgetnum 루틴은 id에 대한 숫자 항목을 가져오거나 해당 항목이 사용 불가능한 경우 -1을 반환한다.    
  * >tgetstr 루틴은 id에 대한 문자열 항목을 반환하거나 해당 항목이 사용 불가능한 경우 0을 반환한다.  
     반환된 문자열을 출력하기 위해 tputs를 사용해라.  
     반환 값은 또한 area가 가리키는 버퍼에 복사되며, 이 값 뒤에 있는 널(null)을 가리키도록 area 값이 업데이트된다.  
  * >tgetflag, tgetnum 및 tgetstr의 id 매개변수의 처음 두 문자만 조회된다.  
  * >tgoto 루틴은 주어진 능력(capability)에 매개변수를 즉시 적용합니다. 이 루틴의 출력은 tputs에 전달되어야 한다.  
  * >tputs 루틴에 대한 설명은 curs_terminfo(3X) 매뉴얼 페이지에 있다. 이는 termcap 또는 terminfo 이름으로 능력을 검색할 수 있다.  

  >   변수 PC, UP 및 BC는 tgetent에 의해 pad_char, cursor_up 및 backspace_if_not_bs에 대한 terminfo 항목 데이터로 설정된다.  
  >   UP는 ncurses에서 사용되지 않는다.  
  >   PC는 tdelay_output 함수에서 사용된다.  
  >   BC는 tgoto 에뮬레이션에서 사용된다.  
  >   ospeed 변수는 시스템별 코딩을 사용하여 터미널 속도를 반영하기 위해 ncurses에 의해 설정된다.  
  
  반환 값  
  >   명시적으로 언급되지 않은 경우, 정수를 반환하는 루틴은 실패 시 ERR을 반환하고 성공 시 OK를 반환한다 (SVr4에서는 "ERR이 아닌 정수 값"만을 명시한다).  
  >   포인터를 반환하는 루틴은 오류 발생 시 NULL을 반환한다.  
