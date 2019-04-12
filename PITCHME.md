# C++
## introduction

---

@snap[north-west]
#### Table of contents
@snapend

@snap[west span-55]
@ul[spaced text-white]

- Special member functions
- Allocation and alignment
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
@snap[north span-100]
## And seventh...
@snapend
+++ 
@snap[north span-100]
## And seventh...
@snapend
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

+++
## So what is going on while deleting polymorphic derived object?
@snap[center span-100]
@ol
1. Virtual deleting destructor is called
2. Base object destructor is called
3. Base class' base object destructor is called
4. Derived class' operator delete is called
@olend
@snapend

---

@snap[north-west]
#### Table of contents
@snapend

@snap[west span-55]
@ul[spaced text-white]

- Special member functions
- Allocation and alignment
- Conversions
- Operator overloading
- Types of inheritance
- Compiler optimizations

@ulend
@snapend

+++
## Data Alignment

> Data alignment means putting
> the data at a memory offset equal to some multiple of the word size, which increases the
> system's performance due to the way the CPU handles memory.

+++
## Data alignment

```cpp
struct A
{
	char c1;
	int a1;
	char c2;
	int a2;
};
//sizeof(A) = 16
//alignof(A) = 4

struct A
{
	char c1; //1 byte
	char padding1[3]; //3 bytes padding
	int a1; //4 bytes
	char c2; //1 byte
	char padding2[3]; //3 bytes padding
	int a2; //4 bytes
};
```
@[1-9]
@[11-19]

+++

## Data alignment

```cpp
struct B
{
	char c1;
	char c2;
	int a1;
	int a2;
};
//sizeof(B) = 12
//alignof(B) = 4

struct B
{
	char c1; //1 byte
	char c2; //1 byte
	char padding[2]; //2 bytes padding
	int a1; //4 bytes
	int a2; //4 bytes
};
```
@[1-9]
@[11-18]

+++

## Data alignment

```cpp
#pragma pack(push, 1)
struct C
{
	char c1;
	int a1;
	char c2;
	int a2;
};
#pragma pack(pop)

//sizeof(C) = 10
//alignof(B) = 1

struct C
{
	char c1; //1 byte
	char c2; //1 byte
	int a1; //4 bytes
	int a2; //4 bytes
};
```
@[1-12]
@[14-20]

+++
## Data alignment

@snap[west span-55]
@ul[text-white]
- order of members matters
- using #pragma pack may harm the performance
- know memory alignment of your target architecture
@ulend
@snapend