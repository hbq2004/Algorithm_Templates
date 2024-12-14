# Fixed Modulus

```cpp
template <class T>
constexpr T power(T a, long long b) {
    T res = 1;
    for (; b; b /= 2, a *= a) {
        if (b & 1) {
            res *= a;
        }
    }
    return res;
}

template <class T, T P>
class ModuloInteger {
    T x;
    static T Mod;
    
    using i64 = long long;

    static constexpr int multip(int a, int b, const int Mod) {
        return 1LL * a * b % Mod;
    }

    static constexpr i64 multip(i64 a, i64 b, const i64 p) {
        i64 res = a * b - i64(1.L * a * b / p) * p;
        res %= p;
        if (res < 0) {
            res += p;
        }
        return res;
    }

    constexpr T norm(T x) const {
		return (x < 0 ? x + getMod() : (x >= getMod() ? x - getMod() : x));
    }

public:
    typedef T ValueType;

    constexpr ModuloInteger() : x{} {}
    constexpr ModuloInteger(i64 x) : x{norm(x % getMod())} {}

    static constexpr T getMod() {
		return (P > 0 ? P : Mod);
    }

    static constexpr void setMod(T Mod_) {
        Mod = Mod_;
    }

    constexpr T val() const {
        return x;
    }

    explicit constexpr operator T() const {
        return x;
    }

    constexpr ModuloInteger operator-() const {
        return ModuloInteger(getMod() - x);
    }

    constexpr ModuloInteger power(i64 m) const {
        if (m < 0) {
            return ::power(inv(), -m);
        }
        return ::power((*this), m);
    }

    constexpr ModuloInteger inv() const {
        assert(x != 0);
        return this->power(getMod() - 2);
    }

    constexpr ModuloInteger &operator*=(ModuloInteger rhs) & {
        x = multip(x, rhs.x, getMod());
        return *this;
    }
    constexpr ModuloInteger &operator+=(ModuloInteger rhs) & {
        x = norm(x + rhs.x);
        return *this;
    }
    constexpr ModuloInteger &operator-=(ModuloInteger rhs) & {
        x = norm(x - rhs.x);
        return *this;
    }
    constexpr ModuloInteger &operator/=(ModuloInteger rhs) & {
        return *this *= rhs.inv();
    }

    friend constexpr ModuloInteger operator+(ModuloInteger lhs, ModuloInteger rhs) {
        return lhs += rhs;
    }

    friend constexpr ModuloInteger operator-(ModuloInteger lhs, ModuloInteger rhs) {
        return lhs -= rhs;
    }

    friend constexpr ModuloInteger operator*(ModuloInteger lhs, ModuloInteger rhs) {
        return lhs *= rhs;
    }

    friend constexpr ModuloInteger operator/(ModuloInteger lhs, ModuloInteger rhs) {
        return lhs /= rhs;
    }

    friend constexpr std::istream &operator>>(std::istream &is, ModuloInteger &a) {
        i64 v;
        is >> v;
        a = ModuloInteger(v);
        return is;
    }

    friend constexpr std::ostream &operator<<(std::ostream &os, const ModuloInteger &a) {
        return os << a.val();
    }

    friend constexpr bool operator==(ModuloInteger lhs, ModuloInteger rhs) {
        return lhs.val() == rhs.val();
    }

    friend constexpr bool operator!=(ModuloInteger lhs, ModuloInteger rhs) {
        return lhs.val() != rhs.val();
    }
};

template <>
int ModuloInteger<int, 0>::Mod = 998244353;

template <>
long long ModuloInteger<long long, 0>::Mod = 4'179'340'454'199'820'289;

constexpr int P = 998'244'353;

using Z = ModuloInteger<int, P>;
```

