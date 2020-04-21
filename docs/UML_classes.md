### Overall 总览

```mermaid
graph TD
X[文本编辑器] --> I(输入监听与处理)
	I --> I1(鼠标输入)
	I --> I2(键盘输入)
		I2 --> I21(快捷键)
X --> B(储存)
	B --> B1(运行时)
		B1 --> B11(光标位置)
		B1 --> B12(选中区间)
	B --> B2(文件)
X --> V(视图)
	V --> V1(文件树)
	V --> V2(编辑区)
		V2 --> V21(行标)
		V2 --> V22(高亮代码)
		V2 --> V23(高亮选区)
	V --> V3(菜单栏)
X --> C(处理函数)
```

### Input 输入

```mermaid
classDiagram
class KeyboardScanner {
	+eventListener(option,callback)
}
class Curse {
	+eventListener(option,callback)
}

```

### Text Buffer 文本缓存

这里有几种策略

- [Gap Buffer](https://en.wikipedia.org/wiki/Gap_buffer) 与[Flexichain理论实现](https://www.common-lisp.net/project/flexichain/download/StrandhVilleneuveMoore.pdf)
- [Rope](https://en.wikipedia.org/wiki/Rope_(data_structure))
- Piece Table (主流数据结构)

本项目可使用单行Gap Buffer+TECO惰性读取的手法减少缓存复制开销。

把文本按行分成一个链表，光标所在的那一行是Gap Buffer，其他行是直接存的字符串。

```mermaid
classDiagram
FileBuffer *--TextBuffer
class FileBuffer {
	+TextBuffer* curBuff
	+TextBuffer* textBuffers
	+<T> metainfo
	+load(path)
}
class TextBuffer{
	-char* buffer
	+int offset
	+insert(str,offs)
	+delete(offs,length)
	+getBuffer()
}
```



### Template

1. [Atom/Atom](https://github.com/atom/atom)
2. [Zedapp/Zed](https://github.com/zedapp/zed)

### References

1. Rob Pike, ‘The Text Editor,‘ [Sam](http://doc.cat-v.org/plan_9/4th_edition/papers/sam/).
2. Charles Crowley, [Data Structures for Text Sequences](https://www.cs.unm.edu/~crowley/papers/sds/sds.html).
