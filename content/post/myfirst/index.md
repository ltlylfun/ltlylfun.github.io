---
title: 第一次写 blog
description: 记录我的第一篇博客文章
date: 2025-02-08T19:54:09+08:00
comments: true
---

## 为什么要搭建个人博客

很早就有搭建个人博客的想法，主要是想有一个属于自己的空间，可以记录学习笔记、分享经验和想法。经过一段时间的准备，终于开始实践这个计划了。

希望通过写博客的方式，能够督促自己持续学习和思考。也欢迎大家多多交流指教！

测试

```
        highlights.forEach(highlight => {
            const copyButton = document.createElement('button');
            copyButton.innerHTML = copyText;
            copyButton.classList.add('copyCodeButton');
            highlight.appendChild(copyButton);

            const codeBlock = highlight.querySelector('code[data-lang]');
            if (!codeBlock) return;

            copyButton.addEventListener('click', () => {
                // 创建一个临时容器来克隆代码块的内容
                const tempCodeBlock = codeBlock.cloneNode(true) as HTMLElement;

                // 删除行号，行号的元素是 <span class="ln">
                const lineNumbers = tempCodeBlock.querySelectorAll('.ln');
                lineNumbers.forEach(lineNumber => lineNumber.remove());

                // 获取没有行号的纯文本内容
                const codeText = tempCodeBlock.textContent;

                navigator.clipboard.writeText(codeText || '')
                // navigator.clipboard.writeText(codeBlock.textContent)
                    .then(() => {
                        copyButton.textContent = copiedText;

                        setTimeout(() => {
                            copyButton.textContent = copyText;
                        }, 1000);
                    })
                    .catch(err => {
                        alert(err)
                        console.log('Something went wrong', err);
                    });
            });
        });

```
