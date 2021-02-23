# PS1

Cuando lo ejecutamos de forma interactiva, y cuando está preparado para leer un comando, bash nos muestra el indicador primario ```PS1```. El indicador secundario ```PS2``` aparece cuando necesita más información para completar un comando.

Bash nos permite personalizar las cadenas de mensajes, insertando barra invertida y caracteres especiales. En la siguiente tabla vemos como se decodifican los caracteres.

| Carácter | Que mensaje insertan |
|----------|----------------------|
| \a       | an ASCII bell character (07)        | 
| \d       | the date in "Weekday Month Date" format (e.g., "Tue May 26")       | 
| \D       | the format is passed to strftime(3) and the result is inserted into the prompt string; an empty format results in a locale-specific time representation. The braces are required       | 
| \e       | an ASCII escape character (033)       | 
| \h       | the hostname up to the first `.'       | 
| \H       | the hostname       | 
| \j       | the number of jobs currently managed by the shell       | 
| \l       | the basename of the shell's terminal device name       | 
| \n       | newline       | 
| \r       | carriage return       | 
| \s       | the name of the shell, the basename of $0 (the portion following the final slash)       | 
| \t       | the current time in 24-hour HH:MM:SS format       | 
| \T       | the current time in 12-hour HH:MM:SS format       | 
| \@       | the current time in 12-hour am/pm format       | 
| \A       | the current time in 24-hour HH:MM format       | 
| \u       | the username of the current user       | 
| \v       | the version of bash (e.g., 4.3)       | 
| \V       | the release of bash, version + patch level (e.g., 4.3.48)       | 
| \w       | the current working directory, with $HOME abbreviated with a tilde (uses the value of the PROMPT_DIRTRIM variable)       | 
| \W       | the basename of the current working directory, with $HOME abbreviated with a tilde       | 
| \!       | the history number of this command       | 
| \#       | the command number of this command       | 
| \$       | if the effective UID is 0, a #, otherwise a $       | 
| \nnn     | the character corresponding to the octal number nnn       | 
| \| \     | a backslash       | 
| \[       | begin a sequence of non-printing characters, which could be used to embed a terminal control sequence into the prompt       | 