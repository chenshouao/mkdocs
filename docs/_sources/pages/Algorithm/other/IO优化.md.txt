# 快速IO

数据IO规模为1e6以上时优化明显。

## 1.OI选手喜欢用，但是优化效果不怎么样的版本

```cpp
namespace IO {
    template <typename T>
    inline void w(T x) { if (x > 9) w(x / 10); putchar(x % 10 + 48); }
    template <typename T>
    inline void write(T x, char c) { if(x < 0) putchar('-'), x = -x; w(x); putchar(c); }
    template <typename T>
    inline void read(T &x) {
        x = 0; T f = 1; char c = getchar();
        for (; !isdigit(c); c = getchar()) if (c == '-') f = -1;
        for (; isdigit(c); c = getchar()) x = (x << 1) + (x << 3) + (c ^ 48);
        x *= f;
    }
}
```

## 2.Neal.Wu用的逆天版本，效率极高

```cpp
/***
date : 2021-09-22
author : csa
***/
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
namespace IO {
    const int BUFFER_SIZE = 1 << 15;

    char input_buffer[BUFFER_SIZE];
    size_t input_pos = 0, input_len = 0;

    char output_buffer[BUFFER_SIZE];
    int output_pos = 0;

    char number_buffer[100];
    uint8_t lookup[100];

    void _update_input_buffer() {
        input_len = fread(input_buffer, sizeof(char), BUFFER_SIZE, stdin);
        input_pos = 0;

        if (input_len == 0)
            input_buffer[0] = EOF;
    }

    inline char next_char(bool advance = true) {
        if (input_pos >= input_len)
            _update_input_buffer();

        return input_buffer[advance ? input_pos++ : input_pos];
    }

    inline bool isspace(char c) {
        return (unsigned char) (c - '\t') < 5 | c == ' ';
    }

    inline bool input_finished() {
        while (isspace(next_char(false)) && input_len > 0)
            next_char();

        return input_len < BUFFER_SIZE && input_pos >= input_len;
    }

    inline void read_char(char &c) {
        while (isspace(next_char(false)))
            next_char();

        c = next_char();
    }

    template<typename T>
    inline void read_int(T &number) {
        bool negative = false;
        number = 0;

        while (!isdigit(next_char(false)))
            if (next_char() == '-')
                negative = true;

        do {
            number = 10 * number + (next_char() - '0');
        } while (isdigit(next_char(false)));

        if (negative)
            number = -number;
    }

    template<typename T, typename... Args>
    inline void read_int(T &number, Args &... args) {
        read_int(number);
        read_int(args...);
    }

    inline void read_double(double &number) {
        bool negative = false;
        number = 0;

        while (!isdigit(next_char(false)))
            if (next_char() == '-')
                negative = true;

        do {
            number = 10 * number + (next_char() - '0');
        } while (isdigit(next_char(false)));

        if (next_char(false) == '.') {
            next_char();

            for (double multiplier = 0.1; isdigit(next_char(false)); multiplier *= 0.1)
                number += multiplier * (next_char() - '0');
        }

        if (negative)
            number = -number;
    }

    inline void read_str(string &str) {
        while (isspace(next_char(false)))
            next_char();

        str.clear();

        do {
            str += next_char();
        } while (!isspace(next_char(false)));
    }

    inline void read_line(string &str) {
        while (next_char(false) == '\n')
            next_char();

        str.clear();

        do {
            str += next_char();
        } while (next_char(false) != '\n');
    }

    void _flush_output() {
        fwrite(output_buffer, sizeof(char), output_pos, stdout);
        output_pos = 0;
    }

    inline void write_char(char c) {
        if (output_pos == BUFFER_SIZE)
            _flush_output();

        output_buffer[output_pos++] = c;
    }

    template<typename T>
    inline void write_int(T number, char after = '\0') {
        if (number < 0) {
            write_char('-');
            number = -number;
        }

        int length = 0;

        while (number >= 10) {
            uint8_t lookup_value = lookup[number % 100];
            number /= 100;
            number_buffer[length++] = char((lookup_value & 15) + '0');
            number_buffer[length++] = char((lookup_value >> 4) + '0');
        }

        if (number != 0 || length == 0)
            write_char(char(number + '0'));

        for (int i = length - 1; i >= 0; i--)
            write_char(number_buffer[i]);

        if (after)
            write_char(after);
    }

    inline void write_str(const string &str, char after = '\0') {
        for (char c : str)
            write_char(c);

        if (after)
            write_char(after);
    }

    inline void write_double(double number, char after = '\0', int places = 6) {
        if (number < 0) {
            write_char('-');
            number = -number;
        }

        assert(number <= 9e18);

        // Round up the number according to places.
        number += 0.5 * pow(0.1, places);
        int64_t floored = int64_t(number);

        if (floored <= numeric_limits<int>::max())
            write_int(int(floored));
        else
            write_int(floored);

        number -= double(floored);

        if (number < 0 || number >= 1)
            number = 0;

        if (places > 0) {
            write_char('.');

            while (places >= 2) {
                number *= 100;
                int two = int(number);
                number -= two;
                uint8_t lookup_value = lookup[two];
                write_char(char((lookup_value >> 4) + '0'));
                write_char(char((lookup_value & 15) + '0'));
                places -= 2;
            }

            if (places == 1) {
                number *= 10;
                int one = int(number);
                write_char(char(one + '0'));
            }
        }

        if (after)
            write_char(after);
    }

    void init() {
        // Ensures that _flush_output() is called at the end of the program.
        bool exit_success = atexit(_flush_output) == 0;
        assert(exit_success);

        for (int i = 0; i < 100; i++)
            lookup[i] = uint8_t((i / 10 << 4) + i % 10);
    }
}

using namespace IO;
int main() {
    /***
    1.必须调用init()
    2.自测的时候必须输入ctrl + z模拟文件尾
    3.字符串仅支持string类
    ***/
    init();
    int n;
    read_int(n);
    write_int(n,'\n');
    double d;
    read_double(d);
    write_double(d, '\n', 12);
    return 0;
}
```