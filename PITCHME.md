# C++
## introduction

---

@snap[north-west]
#### Table of contents
@snapend

@snap[west span-55]
@ul[spaced text-white]

- Special member functions
- Allocation
- Conversions
- Operator overloading
- Types of inheritance
- Compiler optimizations

@ulend
@snapend

+++
@title[Special member functions]

@snap[north span-100]
## Special member functions
@snapend

@snap[west span-55]
Special member functions are member functions that are implicitly defined as member of classes under certain circumstances.
@snapend

@snap[east span-45]
There are six:
@ul
- Default constructor
- Destructor
- Copy constructor
- Copy assignment
- Move constructor
- Move assignment
@ulend
@snapend


+++ 
## And seventh...

+++ 
## And seventh...

### Deleting destructor

+++
### This code:
```cpp
class Base
{
  public:
  virtual ~Base(){}
};

class Foo : public Base
{
};

int main()
{   
     Base* foo = new Foo;
     delete foo;
}
```

+++
### Compiles into (clang 7.0.0):
```x86asm
main:                                   # @main
        push    rbp
        mov     rbp, rsp
        sub     rsp, 32
        mov     eax, 8
        mov     edi, eax
        mov     dword ptr [rbp - 4], 0
        call    operator new(unsigned long)
        mov     rdi, rax
        mov     qword ptr [rbp - 24], rax # 8-byte Spill
        call    Foo::Foo() [base object constructor]
        mov     rax, qword ptr [rbp - 24] # 8-byte Reload
        mov     qword ptr [rbp - 16], rax
        mov     rax, qword ptr [rbp - 16]
        cmp     rax, 0
        mov     qword ptr [rbp - 32], rax # 8-byte Spill
        je      .LBB0_2
        mov     rax, qword ptr [rbp - 32] # 8-byte Reload
        mov     rcx, qword ptr [rax]
        mov     rdi, rax
        call    qword ptr [rcx + 8]
.LBB0_2:
        mov     eax, dword ptr [rbp - 4]
        add     rsp, 32
        pop     rbp
        ret
Foo::Foo() [base object constructor]:                            # @Foo::Foo() [base object constructor]
        push    rbp
        mov     rbp, rsp
        sub     rsp, 16
        mov     qword ptr [rbp - 8], rdi
        mov     rdi, qword ptr [rbp - 8]
        mov     rax, rdi
        mov     qword ptr [rbp - 16], rdi # 8-byte Spill
        mov     rdi, rax
        call    Base::Base() [base object constructor]
        movabs  rax, offset vtable for Foo
        add     rax, 16
        mov     rdi, qword ptr [rbp - 16] # 8-byte Reload
        mov     qword ptr [rdi], rax
        add     rsp, 16
        pop     rbp
        ret
Base::Base() [base object constructor]:                           # @Base::Base() [base object constructor]
        push    rbp
        mov     rbp, rsp
        movabs  rax, offset vtable for Base
        add     rax, 16
        mov     qword ptr [rbp - 8], rdi
        mov     rdi, qword ptr [rbp - 8]
        mov     qword ptr [rdi], rax
        pop     rbp
        ret
Foo::~Foo() [base object destructor]:                            # @Foo::~Foo() [base object destructor]
        push    rbp
        mov     rbp, rsp
        sub     rsp, 16
        mov     qword ptr [rbp - 8], rdi
        mov     rdi, qword ptr [rbp - 8]
        call    Base::~Base() [base object destructor]
        add     rsp, 16
        pop     rbp
        ret
Foo::~Foo() [deleting destructor]:                            # @Foo::~Foo() [deleting destructor]
        push    rbp
        mov     rbp, rsp
        sub     rsp, 16
        mov     qword ptr [rbp - 8], rdi
        mov     rdi, qword ptr [rbp - 8]
        mov     qword ptr [rbp - 16], rdi # 8-byte Spill
        call    Foo::~Foo() [base object destructor]
        mov     rdi, qword ptr [rbp - 16] # 8-byte Reload
        call    operator delete(void*)
        add     rsp, 16
        pop     rbp
        ret
Base::~Base() [base object destructor]:                           # @Base::~Base() [base object destructor]
        push    rbp
        mov     rbp, rsp
        mov     qword ptr [rbp - 8], rdi
        pop     rbp
        ret
Base::~Base() [deleting destructor]:                           # @Base::~Base() [deleting destructor]
        push    rbp
        mov     rbp, rsp
        sub     rsp, 16
        mov     qword ptr [rbp - 8], rdi
        mov     rdi, qword ptr [rbp - 8]
        mov     qword ptr [rbp - 16], rdi # 8-byte Spill
        call    Base::~Base() [base object destructor]
        mov     rdi, qword ptr [rbp - 16] # 8-byte Reload
        call    operator delete(void*)
        add     rsp, 16
        pop     rbp
        ret
vtable for Foo:
        .quad   0
        .quad   0
        .quad   Foo::~Foo() [base object destructor]
        .quad   Foo::~Foo() [deleting destructor]

vtable for Base:
        .quad   0
        .quad   0
        .quad   Base::~Base() [base object destructor]
        .quad   Base::~Base() [deleting destructor]
```
@[102-106]
@[96-100]
@[83-95]
@[64-76]
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
