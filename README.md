## 8queens, C vs. ASM edition

After my [blog post](http://davidad.github.io/blog/2014/02/25/overkilling-the-8-queens-problem/) about this repository made it to [the front page of Hacker News](https://news.ycombinator.com/item?id=7301481), [a challenger appeared](https://news.ycombinator.com/item?id=7302005), wondering if their old C code was really 10 times faster than my handcrafted assembly. I believe we are all glad this is not actually the case, although it appeared so at first. Both my code and bluecalm's code have been tweaked in this branch to do nothing other than solve the 8-queens problem ten thousand times in a row, and return the count of solutions as the final answer. Anyone who wants to repeat the experiment on their own (Sandy Bridge or newer) hardware should be able to clone this branch of the repository as follows:

    $ git clone https://github.com/davidad/8queens.git
    $ cd 8queens
    $ git checkout +c_comparison

and, assuming you have a recent version of `nasm` installed, simply run 

    $ make

You should get a result like this:

    gcc -O3 -march=core2 -msse4.1 -msse4.2 8q_C_bluecalm.c -o 8q_C_bluecalm
    nasm 8q_x64_davidad.asm -DLOOPED=10000 -f macho64 -o 8q_x64_davidad.o
    ld -o 8q_x64_davidad 8q_x64_davidad.o

    time ./8q_C_bluecalm  ; echo $?

    real	0m0.802s
    user	0m0.801s
    sys 	0m0.001s
    92

    time ./8q_x64_davidad ; echo $?

    real	0m0.113s
    user	0m0.112s
    sys 	0m0.000s
    92

bluecalm also suggests that you try replacing `-O3` with `-Ofast` if you have a version of gcc that supports it. (I don't yet.)
