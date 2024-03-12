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
    clear_undo는 내부적으로 유지 중인 undo_list를 초기화할 지의 여부를 결정 짓는 값이다.  
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
  
* wait3
  
* wait4
  
* signal
  
* sigaction
  
* sigemptyset
  
* sigaddset
  
* kill
  
* exit
  
* getcwd
  
* chdir
  
* stat
  
* lstat
  
* fstat
  
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
  
