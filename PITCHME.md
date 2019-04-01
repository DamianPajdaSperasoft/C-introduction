# C++
## introduction

---

@snap[north-west]
#### Table of contents
@snapend

@snap[west span-55]
@ul[spaced text-white]

- User defined types
- Special member functions
- Conversions
- Operator overloading
- Types of inheritance
- Compiler optimizations

@ulend
@snapend

+++
@title[Customize Slide Layout]

@snap[north span-100]
## User defined types
@snapend


```cpp
union Foo
{
	float f;
	int i;
	char ch[4];
};

class Bar 
{
private:
	float f;
	int i;
	char ch[4];
};

struct Baz
{
	float f;
	int i;
	char ch[4];
};
```
@[1-6]
@[8-14]
@[16-21]

---?color=#E58537 
@title[Add A Little Imagination]

@snap[north-west]
#### Add a splash of @color[cyan](**color**) and you are ready to start presenting...
@snapend

@snap[west span-55]
@ul[spaced text-white]
- You will be amazed
- What you can achieve
- *With a little imagination...*
- And **GitPitch Markdown**
@ulend
@snapend

@snap[east span-45]
@img[shadow](assets/img/conference.png)
@snapend

---?image=assets/img/presenter.jpg

@snap[north span-100 headline]
## Now It's Your Turn
@snapend

@snap[south span-100 text-06]
[Click here to jump straight into the interactive feature guides in the GitPitch Docs @fa[external-link]](https://gitpitch.com/docs/getting-started/tutorial/)
@snapend
