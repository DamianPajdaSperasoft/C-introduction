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
@[8-13]
@[15-20]

+++
@snap[north span-100]
Unions
@snapend

@snap[west span-45]
@ul[spaced text-white]
- Non-virtual member functions are allowed
- No inheritance
- Non-static referebce data members are not allowed
@ulend
@snapend


```cpp
union S
{
    std::int32_t n;     // occupies 4 bytes
    std::uint16_t s[2]; // occupies 4 bytes
    std::uint8_t c;     // occupies 1 byte
};                      // the whole union occupies 4 bytes
 
int main()
{
    S s = {0x12345678}; // initializes the first member, s.n is now the active member
    // at this point, reading from s.s or s.c is undefined behavior
    std::cout << std::hex << "s.n = " << s.n << '\n';
    s.s[0] = 0x0011; // s.s is now the active member
    // at this point, reading from n or c is UB but most compilers define it
    std::cout << "s.c is now " << +s.c << '\n' // 11 or 00, depending on platform
              << "s.n is now " << s.n << '\n'; // 12340011 or 00115678
}

union S
{
    std::string str;
    std::vector<int> vec;
    ~S() {} // needs to know which member is active, only possible in union-like class 
};          // the whole union occupies max(sizeof(string), sizeof(vector<int>))
 
int main()
{
    S s = {"Hello, world"};
    // at this point, reading from s.vec is undefined behavior
    std::cout << "s.str = " << s.str << '\n';
    s.str.~basic_string();
    new (&s.vec) std::vector<int>;
    // now, s.vec is the active member of the union
    s.vec.push_back(10);
    std::cout << s.vec.size() << '\n';
    s.vec.~vector();
}
```
@[1-16]
@[18-35]



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
