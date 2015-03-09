---
layout: post
title: 在终端打印树的结构
---

在学习数据结构中的时候，我们往往会把数据结构中的所有数据遍历一遍打印在终端上，以确保数据结构运行正常。对于线性结构来说，直接遍历就行了，然而对于树这种结构就有不同的打印顺序（前序、中序、后序）。使用不同的顺序遍历树可以得到树中的所有元素，但是往往不够直观。我们希望在终端中显示树的拓扑结构，从这个结构就能清楚的看出每个节点的父节点以及子节点。

我在学习的过程中在stackoverflow上面找到了一段大神写的程序，使用这段程序就能很好的把树的拓扑结构给打印出来，这样就能在学习树有关的知识的时候有更直观的认识。

```c

// Codes for printing the tree in Diagram
int _print_t(AvlTree tree, int is_left, int offset, int depth, char s[DEPTH*2][255])
{
    char b[DEPTH*2];
    int width = 5;

    if (!tree) return 0;

    sprintf(b, "(%2d )", tree->data);

    int left  = _print_t(tree->lchild,  1, offset,                depth + 1, s);
    int right = _print_t(tree->rchild, 0, offset + left + width, depth + 1, s);
    int i;

#ifdef COMPACT
    for (i = 0; i < width; i++)
        s[depth][offset + left + i] = b[i];

    if (depth && is_left) {

        for (i = 0; i < width + right; i++)
            s[depth - 1][offset + left + width/2 + i] = '-';

        s[depth - 1][offset + left + width/2] = '.';

    } else if (depth && !is_left) {

        for (i = 0; i < left + width; i++)
            s[depth - 1][offset - width/2 + i] = '-';

        s[depth - 1][offset + left + width/2] = '.';
    }
#else
    for (i = 0; i < width; i++)
        s[2 * depth][offset + left + i] = b[i];

    if (depth && is_left) {

        for (i = 0; i < width + right; i++)
            s[2 * depth - 1][offset + left + width/2 + i] = '-';

        s[2 * depth - 1][offset + left + width/2] = '+';
        s[2 * depth - 1][offset + left + width + right + width/2] = '+';

    } else if (depth && !is_left) {

        for (i = 0; i < left + width; i++)
            s[2 * depth - 1][offset - width/2 + i] = '-';

        s[2 * depth - 1][offset + left + width/2] = '+';
        s[2 * depth - 1][offset - width/2 - 1] = '+';
    }
#endif

    return left + width + right;
}

int print_t(AvlTree tree)
{
    int i;
    char s[DEPTH*2][255];
    for (i = 0; i < DEPTH*2; i++)
        sprintf(s[i], "%80s", " ");

    _print_t(tree, 0, 0, 0, s);

    for (i = 0; i < DEPTH*2; i++)
        printf("%s\n", s[i]);
}

```
