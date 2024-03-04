```C++
void Rational::Set(int64_t numer, int64_t denom) {
    int64_t nod = std::min(std::abs(numer), std::abs(denom));
    int64_t nod1 = std::max(std::abs(denom), std::abs(numer));
    while (nod != 0) {
        int64_t help = 0;
        help = nod;
        nod1 = nod1 % nod;
        nod = help;
        nod = nod1;
        nod1 = help;
    }
    denom_ = denom_ / std::abs(nod1);
    numer_ = numer_ / std::abs(nod1);
}
Rational::Rational() {
    numer_ = 0;
    denom_ = 1;
}
Rational::Rational(int64_t num, int64_t den) {
    if (den == 0) {
        throw RationalDivisionByZero{};
    }
    if (num != 0) {
        if (den >= 0) {
            numer_ = num;
            denom_ = den;
        } else {
            numer_ = -num;
            denom_ = -den;
        }
    } else {
        numer_ = num;
        denom_ = 1;
    }
    Set(numer_, denom_);
}
Rational::Rational(int64_t num) {
    numer_ = num;
    denom_ = 1;
}
Rational::Rational(int num) {
    numer_ = num;
    denom_ = 1;
}
Rational::Rational(size_t num) {
    numer_ = num;  // NOLINT
    denom_ = 1;
}
Rational::Rational(double num) {
    numer_ = round(num * 100000);  // NOLINT
    denom_ = 100000;               // NOLINT
}
int64_t Rational::GetNumerator() const {
    return numer_;
}
int64_t Rational::GetDenominator() const {
    return denom_;
}
void Rational::SetNumerator(int64_t value) {
    numer_ = value;
}
void Rational::SetDenominator(int64_t value) {
    if (value == 0) {
        throw RationalDivisionByZero{};
    }
    if (value < 0) {
        denom_ = -value;
        numer_ = -numer_;
    } else {
        denom_ = value;
    }
}
Rational& operator+=(Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.numer_ * rhs.denom_;
    int64_t b = lhs.denom_ * rhs.numer_;
    Rational ans(a + b, rhs.denom_ * lhs.denom_);
    lhs.numer_ = ans.numer_;
    lhs.denom_ = ans.denom_;
    return lhs;
}
Rational& operator*=(Rational& lhs, const Rational& rhs) {
    if (rhs.GetDenominator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    } else {
        int64_t a = lhs.numer_ * rhs.numer_;
        int64_t b = lhs.denom_ * rhs.denom_;
        Rational ans(a, b);
        lhs.numer_ = ans.numer_;
        lhs.denom_ = ans.denom_;
        return lhs;
    }
}
Rational& operator--(Rational& ratio) {
    Rational ans(ratio.numer_ - ratio.denom_, ratio.denom_);
    ratio.numer_ = ans.numer_;
    ratio.denom_ = ans.denom_;
    return ratio;
}
Rational& operator++(Rational& ratio) {
    Rational ans(ratio.numer_ + ratio.denom_, ratio.denom_);
    ratio.numer_ = ans.numer_;
    ratio.denom_ = ans.denom_;
    return ratio;
}
std::istream& operator>>(std::istream& is, Rational& ratio) {
    if (ratio.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    Rational ans(ratio.numer_, ratio.denom_);
    is >> ans.numer_ >> ans.denom_;
    return is;
}
Rational& operator/=(Rational& lhs, const Rational& rhs) {
    if (rhs.GetNumerator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    int sigh = 1;
    if (lhs.GetNumerator() * rhs.GetNumerator() < 0) {
        sigh = -1;
    }
    int64_t a = std::abs(lhs.GetNumerator()) * std::abs(rhs.GetDenominator()) * sigh;
    int64_t b = std::abs(lhs.GetDenominator()) * std::abs(rhs.GetNumerator());
    Rational ans(a, b);
    lhs.SetNumerator(ans.GetNumerator());
    lhs.SetDenominator(ans.GetDenominator());
    return lhs;
}
bool operator<(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a < b;
}
bool operator>(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a > b;
}
bool operator==(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a == b;
}
bool operator>=(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a >= b;
}
bool operator<=(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a <= b;
}
bool operator!=(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a != b;
}
Rational operator+(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    Rational ans(a + b, rhs.GetDenominator() * lhs.GetDenominator());
    return ans;
}
Rational operator-(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    Rational ans(a - b, rhs.GetDenominator() * lhs.GetDenominator());
    return ans;
}
Rational operator--(Rational& ratio, int) {
    Rational ans(ratio.GetNumerator() - ratio.GetDenominator(), ratio.GetDenominator());
    ratio.SetNumerator(ans.GetNumerator());
    ratio.SetDenominator(ans.GetDenominator());
    return ratio;
}
Rational operator++(Rational& ratio, int) {
    Rational ans(ratio.GetNumerator() + ratio.GetDenominator(), ratio.GetDenominator());
    ratio.SetNumerator(ans.GetNumerator());
    ratio.SetDenominator(ans.GetDenominator());
    return ratio;
}
Rational operator*(const Rational& lhs, const Rational& rhs) {
    if (rhs.GetDenominator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    int64_t a = lhs.GetNumerator() * rhs.GetNumerator();
    int64_t b = lhs.GetDenominator() * rhs.GetDenominator();
    Rational ans(a, b);
    return ans;
}
Rational operator/(const Rational& lhs, const Rational& rhs) {
    if (rhs.GetNumerator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    int sigh = 1;
    if (lhs.GetNumerator() * rhs.GetNumerator() < 0) {
        sigh = -1;
    }
    int64_t a = std::abs(lhs.GetNumerator()) * std::abs(rhs.GetDenominator()) * sigh;
    int64_t b = std::abs(lhs.GetDenominator()) * std::abs(rhs.GetNumerator());
    Rational ans(a, b);
    return ans;
}
std::ostream& operator<<(std::ostream& os, const Rational& ratio) {
    Rational ans(ratio.GetNumerator(), ratio.GetDenominator());
    if (ratio.GetDenominator() != 1 && ratio.GetNumerator() != 0) {
        os << ans.GetNumerator() << "/" << ans.GetDenominator();
    } else {
        os << ans.GetNumerator();
    }
    return os;
}
Rational operator+(const Rational& ratio) {
    Rational ans(ratio.GetNumerator(), ratio.GetDenominator());
    return ans;
}
Rational operator-(const Rational& ratio) {
    Rational ans(-1 * ratio.GetNumerator(), ratio.GetDenominator());
    return ans;
}
Rational& operator-=(Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    Rational ans(a - b, lhs.GetDenominator() * rhs.GetDenominator());
    lhs.SetNumerator(ans.GetNumerator());
    lhs.SetDenominator(ans.GetDenominator());
    return lhs;
}
std::unordered_map<std::string, int> SenIntoWords(std::string& s) {
    std::unordered_map<char, char> a;
    for (char i = 65; i < 91; ++i) {  // NOLINT
        a[i] = i + 32;                // NOLINT
    }
    std::string word;
    std::unordered_map<std::string, int> stoke;
    for (auto& item : s) {
        if (isalpha(item)) {
            if (isupper(item)) {
                word += a[item];
            } else {
                word += item;
            }
        } else {
            if (!empty(word)) {
                stoke[word] += 1;
                word = "";
            }
        }
    }
    if (!empty(word)) {
        stoke[word]++;
    }
    return stoke;
};
std::vector<std::pair<std::string, const char*>> TextToSen(std::string_view& s) {
    std::string sen;
    std::vector<std::pair<std::string, const char*>> k;
    const char* p = s.begin();
    const char* f = s.begin();
    while (p != s.end()) {
        if (*p != '\n') {
            sen += *p;
        } else {
            auto w = std::make_pair(sen, f);
            k.push_back(w);
            sen = "";
            if (p + 1 != s.end()) {
                f = p + 1;
            }
        }
        p++;
    }
    if (!empty(sen)) {
        auto w = std::make_pair(sen, f);
        k.push_back(w);
    }
    return k;
}
std::vector<std::string_view> Search(std::string_view text, std::string_view query, size_t results_count) {
    std::vector<std::unordered_map<std::string, int>> text_words;
    auto k = TextToSen(text);
    auto k1 = TextToSen(query);
    auto t = k1[0].first;
    auto s_w = SenIntoWords(t);
    std::vector<std::vector<Rational>> ans;

    for (size_t i = 0; i < k.size(); ++i) {  // NOLINT
        text_words.push_back(SenIntoWords(k[i].first));
        ans.push_back({0, i});
    }
    for (auto& item : s_w) {
        s_w[item.first] = 0;
    }
    std::vector<int64_t> ct;
    for (size_t i = 0; i < text_words.size(); ++i) {
        int64_t count = 0;
        for (auto& item: text_words[i]) {
            count += item.second;
        }
        ct.push_back(count);
    }
    for (auto& item : s_w) {
        for (size_t i = 0; i < text_words.size(); ++i) {
            if (text_words[i].contains(item.first)) {
                ++s_w[item.first];
            }
        }
        Rational p;
        if (s_w[item.first] != 0) {
            Rational s1 = log2((k.size() * 1.0) / s_w[item.first]);  // NOLINT
            p = s1;
        }
        for (size_t i = 0; i < text_words.size(); ++i) {  // NOLINT
            if (text_words[i].contains(item.first)) {
                ans[i][0] += p * Rational(text_words[i][item.first], ct[i]);  // NOLINT
            }
        }
    }
    std::sort(ans.begin(), ans.end());
    std::vector<std::string_view> ot;
    for (size_t i = ans.size(); i >= 1; --i) {
        if (results_count > 0 && ans[i - 1][0] > 0) {
            std::string_view s2(k[ans[i - 1][1].GetNumerator()].second, k[ans[i - 1][1].GetNumerator()].first.size());
            ot.push_back(s2);
            results_count--;
        }
    }
    return ot;
}
```

``` C++
#include <bits/stdc++.h>
#include "search.h"
void Rational::Set(int64_t numer, int64_t denom) {
    int64_t nod = std::min(std::abs(numer), std::abs(denom));
    int64_t nod1 = std::max(std::abs(denom), std::abs(numer));
    while (nod != 0) {
        int64_t help = 0;
        help = nod;
        nod1 = nod1 % nod;
        nod = help;
        nod = nod1;
        nod1 = help;
    }
    denom_ = denom_ / std::abs(nod1);
    numer_ = numer_ / std::abs(nod1);
}
Rational::Rational() {
    numer_ = 0;
    denom_ = 1;
}
Rational::Rational(int64_t num, int64_t den) {
    if (den == 0) {
        throw RationalDivisionByZero{};
    }
    if (num != 0) {
        if (den >= 0) {
            numer_ = num;
            denom_ = den;
        } else {
            numer_ = -num;
            denom_ = -den;
        }
    } else {
        numer_ = num;
        denom_ = 1;
    }
    Set(numer_, denom_);
}
Rational::Rational(int64_t num) {
    numer_ = num;
    denom_ = 1;
}
Rational::Rational(int num) {
    numer_ = num;
    denom_ = 1;
}
Rational::Rational(size_t num) {
    numer_ = num;  // NOLINT
    denom_ = 1;
}
Rational::Rational(double num) {
    numer_ = round(num * 100);  // NOLINT
    denom_ = 100;               // NOLINT
}
int64_t Rational::GetNumerator() const {
    return numer_;
}
int64_t Rational::GetDenominator() const {
    return denom_;
}
void Rational::SetNumerator(int64_t value) {
    numer_ = value;
}
void Rational::SetDenominator(int64_t value) {
    if (value == 0) {
        throw RationalDivisionByZero{};
    }
    if (value < 0) {
        denom_ = -value;
        numer_ = -numer_;
    } else {
        denom_ = value;
    }
}
Rational& operator+=(Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.numer_ * rhs.denom_;
    int64_t b = lhs.denom_ * rhs.numer_;
    Rational ans(a + b, rhs.denom_ * lhs.denom_);
    lhs.numer_ = ans.numer_;
    lhs.denom_ = ans.denom_;
    return lhs;
}
Rational& operator*=(Rational& lhs, const Rational& rhs) {
    if (rhs.GetDenominator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    } else {
        int64_t a = lhs.numer_ * rhs.numer_;
        int64_t b = lhs.denom_ * rhs.denom_;
        Rational ans(a, b);
        lhs.numer_ = ans.numer_;
        lhs.denom_ = ans.denom_;
        return lhs;
    }
}
Rational& operator--(Rational& ratio) {
    Rational ans(ratio.numer_ - ratio.denom_, ratio.denom_);
    ratio.numer_ = ans.numer_;
    ratio.denom_ = ans.denom_;
    return ratio;
}
Rational& operator++(Rational& ratio) {
    Rational ans(ratio.numer_ + ratio.denom_, ratio.denom_);
    ratio.numer_ = ans.numer_;
    ratio.denom_ = ans.denom_;
    return ratio;
}
std::istream& operator>>(std::istream& is, Rational& ratio) {
    if (ratio.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    Rational ans(ratio.numer_, ratio.denom_);
    is >> ans.numer_ >> ans.denom_;
    return is;
}
Rational& operator/=(Rational& lhs, const Rational& rhs) {
    if (rhs.GetNumerator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    int sigh = 1;
    if (lhs.GetNumerator() * rhs.GetNumerator() < 0) {
        sigh = -1;
    }
    int64_t a = std::abs(lhs.GetNumerator()) * std::abs(rhs.GetDenominator()) * sigh;
    int64_t b = std::abs(lhs.GetDenominator()) * std::abs(rhs.GetNumerator());
    Rational ans(a, b);
    lhs.SetNumerator(ans.GetNumerator());
    lhs.SetDenominator(ans.GetDenominator());
    return lhs;
}
bool operator<(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a < b;
}
bool operator>(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a > b;
}
bool operator==(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a == b;
}
bool operator>=(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a >= b;
}
bool operator<=(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a <= b;
}
bool operator!=(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a != b;
}
Rational operator+(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    Rational ans(a + b, rhs.GetDenominator() * lhs.GetDenominator());
    return ans;
}
Rational operator-(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    Rational ans(a - b, rhs.GetDenominator() * lhs.GetDenominator());
    return ans;
}
Rational operator--(Rational& ratio, int) {
    Rational ans(ratio.GetNumerator() - ratio.GetDenominator(), ratio.GetDenominator());
    ratio.SetNumerator(ans.GetNumerator());
    ratio.SetDenominator(ans.GetDenominator());
    return ratio;
}
Rational operator++(Rational& ratio, int) {
    Rational ans(ratio.GetNumerator() + ratio.GetDenominator(), ratio.GetDenominator());
    ratio.SetNumerator(ans.GetNumerator());
    ratio.SetDenominator(ans.GetDenominator());
    return ratio;
}
Rational operator*(const Rational& lhs, const Rational& rhs) {
    if (rhs.GetDenominator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    int64_t a = lhs.GetNumerator() * rhs.GetNumerator();
    int64_t b = lhs.GetDenominator() * rhs.GetDenominator();
    Rational ans(a, b);
    return ans;
}
Rational operator/(const Rational& lhs, const Rational& rhs) {
    if (rhs.GetNumerator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    int sigh = 1;
    if (lhs.GetNumerator() * rhs.GetNumerator() < 0) {
        sigh = -1;
    }
    int64_t a = std::abs(lhs.GetNumerator()) * std::abs(rhs.GetDenominator()) * sigh;
    int64_t b = std::abs(lhs.GetDenominator()) * std::abs(rhs.GetNumerator());
    Rational ans(a, b);
    return ans;
}
std::ostream& operator<<(std::ostream& os, const Rational& ratio) {
    Rational ans(ratio.GetNumerator(), ratio.GetDenominator());
    if (ratio.GetDenominator() != 1 && ratio.GetNumerator() != 0) {
        os << ans.GetNumerator() << "/" << ans.GetDenominator();
    } else {
        os << ans.GetNumerator();
    }
    return os;
}
Rational operator+(const Rational& ratio) {
    Rational ans(ratio.GetNumerator(), ratio.GetDenominator());
    return ans;
}
Rational operator-(const Rational& ratio) {
    Rational ans(-1 * ratio.GetNumerator(), ratio.GetDenominator());
    return ans;
}
Rational& operator-=(Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    Rational ans(a - b, lhs.GetDenominator() * rhs.GetDenominator());
    lhs.SetNumerator(ans.GetNumerator());
    lhs.SetDenominator(ans.GetDenominator());
    return lhs;
}
std::unordered_map<std::string, int> SenIntoWords(std::string& s) {
    std::unordered_map<char, char> a;
    for (char i = 65; i < 91; ++i) {  // NOLINT
        a[i] = i + 32;                // NOLINT
    }
    std::string word;
    std::unordered_map<std::string, int> stoke;
    for (auto& item : s) {
        if (isalpha(item)) {
            if (isupper(item)) {
                word += a[item];
            } else {
                word += item;
            }
        } else {
            if (!empty(word)) {
                stoke[word] += 1;
                word = "";
            }
        }
    }
    if (!empty(word)) {
        stoke[word]++;
    }
    return stoke;
};
std::vector<std::pair<std::string, const char*>> TextToSen(std::string_view& s) {
    std::string sen;
    std::vector<std::pair<std::string, const char*>> k;
    const char* p = s.begin();
    const char* f = s.begin();
    while (p != s.end()) {
        if (*p != '\n') {
            sen += *p;
        } else {
            auto w = std::make_pair(sen, f);
            k.push_back(w);
            sen = "";
            if (p + 1 != s.end()) {
                f = p + 1;
            }
        }
        p++;
    }
    if (!empty(sen)) {
        auto w = std::make_pair(sen, f);
        k.push_back(w);
    }
    return k;
}
std::vector<std::string_view> Search(std::string_view text, std::string_view query, size_t results_count) {
    std::vector<std::unordered_map<std::string, int>> text_words;
    auto k = TextToSen(text);
    auto k1 = TextToSen(query);
    auto t = k1[0].first;
    auto s_w = SenIntoWords(t);
    std::vector<std::vector<Rational>> ans;

    for (size_t i = 0; i < k.size(); ++i) {  // NOLINT
        text_words.push_back(SenIntoWords(k[i].first));
        ans.push_back({0, i});
    }
    for (auto& item : s_w) {
        s_w[item.first] = 0;
    }
    for (auto& item : s_w) {
        for (size_t i = 0; i < text_words.size(); ++i) {
            if (text_words[i].contains(item.first)) {
                ++s_w[item.first];
            }
        }
        Rational p;
        if (s_w[item.first] != 0) {
            auto s1 = log(k.size() / s_w[item.first]);  // NOLINT
            p = s1;
        }
        for (size_t i = 0; i < text_words.size(); ++i) {  // NOLINT
            if (text_words[i].contains(item.first)) {
                ans[i][0] += p * Rational(text_words[i][item.first], text_words[i].size());  // NOLINT
            }
        }
    }
    std::sort(ans.begin(), ans.end());
    std::vector<std::string_view> ot;
    for (size_t i = ans.size(); i >= 1; --i) {
        if (results_count > 0 && ans[i - 1][0] > 0) {
            std::string_view s2(k[ans[i - 1][1].GetNumerator()].second, k[ans[i - 1][1].GetNumerator()].first.size());
            ot.push_back(s2);
            results_count--;
        }
    }
    std::reverse(std::begin(ot), std::end(ot));
    return ot;
}
```
``` C++
#pragma once

#include <stdexcept>
#include <iostream>
#include <numeric>
#include <cstdint>

class RationalDivisionByZero : public std::runtime_error {
public:
    RationalDivisionByZero() : std::runtime_error("RationalDivisionByZero") {
    }
};

class Rational {
public:
    Rational();

    Rational(int64_t value);  // NOLINT
    Rational(int num);  // NOLINT
    Rational(int64_t numer, int64_t denom);
    Rational(double num); // NOLINT
    int64_t GetNumerator() const;

    int64_t GetDenominator() const;

    void SetNumerator(int64_t value);

    void SetDenominator(int64_t value);

    friend Rational& operator+=(Rational& lhs, const Rational& rhs);

    friend Rational& operator*=(Rational& lhs, const Rational& rhs);

    friend Rational& operator++(Rational& ratio);  // faster than += 1

    friend Rational& operator--(Rational& ratio);

    friend std::istream& operator>>(std::istream& is, Rational& ratio);

private:
    void Set(int64_t numer, int64_t denom);

    int64_t numer_;
    int64_t denom_;
};

Rational operator+(const Rational& ratio);

Rational operator-(const Rational& ratio);

Rational& operator-=(Rational& lhs, const Rational& rhs);

Rational& operator/=(Rational& lhs, const Rational& rhs);

Rational operator+(const Rational& lhs, const Rational& rhs);

Rational operator-(const Rational& lhs, const Rational& rhs);

Rational operator*(const Rational& lhs, const Rational& rhs);

Rational operator/(const Rational& lhs, const Rational& rhs);

Rational operator++(Rational& ratio, int);

Rational operator--(Rational& ratio, int);

bool operator<(const Rational& lhs, const Rational& rhs);

bool operator>(const Rational& lhs, const Rational& rhs);

bool operator<=(const Rational& lhs, const Rational& rhs);

bool operator>=(const Rational& lhs, const Rational& rhs);

bool operator==(const Rational& lhs, const Rational& rhs);

bool operator!=(const Rational& lhs, const Rational& rhs);

std::ostream& operator<<(std::ostream& os, const Rational& ratio);
```
``` C++
#include<bits/stdc++.h>
#include "rational.h"
void Rational::Set(int64_t numer, int64_t denom) {
    int64_t nod = std::min(std::abs(numer), std::abs(denom));
    int64_t nod1 = std::max(std::abs(denom), std::abs(numer));
    while (nod != 0) {
        int64_t help = 0;
        help = nod;
        nod1 = nod1 % nod;
        nod = help;
        nod = nod1;
        nod1 = help;
    }
    denom_ = denom_ / std::abs(nod1);
    numer_ = numer_ / std::abs(nod1);
}
Rational::Rational() {
    numer_ = 0;
    denom_ = 1;
}
Rational::Rational(int64_t num, int64_t den) {
    if (den == 0) {
        throw RationalDivisionByZero{};
    }
    if (num != 0) {
        if (den >= 0) {
            numer_ = num;
            denom_ = den;
        } else {
            numer_ = -num;
            denom_ = -den;
        }
    } else {
        numer_ = num;
        denom_ = 1;
    }
    Set(numer_, denom_);
}
Rational::Rational(int64_t num) {
    numer_ = num;
    denom_ = 1;
}
Rational::Rational(int num) {
    numer_ = num;
    denom_ = 1;
}
Rational::Rational(double num) {
    numer_ = round(num * 100);
    denom_ = 100;
}
int64_t Rational::GetNumerator() const {
    return numer_;
}
int64_t Rational::GetDenominator() const {
    return denom_;
}
void Rational::SetNumerator(int64_t value) {
    numer_ = value;
}
void Rational::SetDenominator(int64_t value) {
    if (value == 0) {
        throw RationalDivisionByZero{};
    }
    if (value < 0) {
        denom_ = -value;
        numer_ = -numer_;
    } else {
        denom_ = value;
    }
}
Rational& operator+=(Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.numer_ * rhs.denom_;
    int64_t b = lhs.denom_ * rhs.numer_;
    Rational ans(a + b, rhs.denom_ * lhs.denom_);
    lhs.numer_ = ans.numer_;
    lhs.denom_ = ans.denom_;
    return lhs;
}
Rational& operator*=(Rational& lhs, const Rational& rhs) {
    if (rhs.GetDenominator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    } else {
        int64_t a = lhs.numer_ * rhs.numer_;
        int64_t b = lhs.denom_ * rhs.denom_;
        Rational ans(a, b);
        lhs.numer_ = ans.numer_;
        lhs.denom_ = ans.denom_;
        return lhs;
    }
}
Rational& operator--(Rational& ratio) {
    Rational ans(ratio.numer_ - ratio.denom_, ratio.denom_);
    ratio.numer_ = ans.numer_;
    ratio.denom_ = ans.denom_;
    return ratio;
}
Rational& operator++(Rational& ratio) {
    Rational ans(ratio.numer_ + ratio.denom_, ratio.denom_);
    ratio.numer_ = ans.numer_;
    ratio.denom_ = ans.denom_;
    return ratio;
}
std::istream& operator>>(std::istream& is, Rational& ratio) {
    if (ratio.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    Rational ans(ratio.numer_, ratio.denom_);
    is >> ans.numer_ >> ans.denom_;
    return is;
}
Rational& operator/=(Rational& lhs, const Rational& rhs) {
    if (rhs.GetNumerator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    int sigh = 1;
    if (lhs.GetNumerator() * rhs.GetNumerator() < 0) {
        sigh = -1;
    }
    int64_t a = std::abs(lhs.GetNumerator()) * std::abs(rhs.GetDenominator()) * sigh;
    int64_t b = std::abs(lhs.GetDenominator()) * std::abs(rhs.GetNumerator());
    Rational ans(a, b);
    lhs.SetNumerator(ans.GetNumerator());
    lhs.SetDenominator(ans.GetDenominator());
    return lhs;
}
bool operator<(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a < b;
}
bool operator>(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a > b;
}
bool operator==(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a == b;
}
bool operator>=(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a >= b;
}
bool operator<=(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a <= b;
}
bool operator!=(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a != b;
}
Rational operator+(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    Rational ans(a + b, rhs.GetDenominator() * lhs.GetDenominator());
    return ans;
}
Rational operator-(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    Rational ans(a - b, rhs.GetDenominator() * lhs.GetDenominator());
    return ans;
}
Rational operator--(Rational& ratio, int) {
    Rational ans(ratio.GetNumerator() - ratio.GetDenominator(), ratio.GetDenominator());
    ratio.SetNumerator(ans.GetNumerator());
    ratio.SetDenominator(ans.GetDenominator());
    return ratio;
}
Rational operator++(Rational& ratio, int) {
    Rational ans(ratio.GetNumerator() + ratio.GetDenominator(), ratio.GetDenominator());
    ratio.SetNumerator(ans.GetNumerator());
    ratio.SetDenominator(ans.GetDenominator());
    return ratio;
}
Rational operator*(const Rational& lhs, const Rational& rhs) {
    if (rhs.GetDenominator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    int64_t a = lhs.GetNumerator() * rhs.GetNumerator();
    int64_t b = lhs.GetDenominator() * rhs.GetDenominator();
    Rational ans(a, b);
    return ans;
}
Rational operator/(const Rational& lhs, const Rational& rhs) {
    if (rhs.GetNumerator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    int sigh = 1;
    if (lhs.GetNumerator() * rhs.GetNumerator() < 0) {
        sigh = -1;
    }
    int64_t a = std::abs(lhs.GetNumerator()) * std::abs(rhs.GetDenominator()) * sigh;
    int64_t b = std::abs(lhs.GetDenominator()) * std::abs(rhs.GetNumerator());
    Rational ans(a, b);
    return ans;
}
std::ostream& operator<<(std::ostream& os, const Rational& ratio) {
    Rational ans(ratio.GetNumerator(), ratio.GetDenominator());
    if (ratio.GetDenominator() != 1 && ratio.GetNumerator() != 0) {
        os << ans.GetNumerator() << "/" << ans.GetDenominator();
    } else {
        os << ans.GetNumerator();
    }
    return os;
}
Rational operator+(const Rational& ratio) {
    Rational ans(ratio.GetNumerator(), ratio.GetDenominator());
    return ans;
}
Rational operator-(const Rational& ratio) {
    Rational ans(-1 * ratio.GetNumerator(), ratio.GetDenominator());
    return ans;
}
Rational& operator-=(Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    Rational ans(a - b, lhs.GetDenominator() * rhs.GetDenominator());
    lhs.SetNumerator(ans.GetNumerator());
    lhs.SetDenominator(ans.GetDenominator());
    return lhs;
}


std::unordered_map<std::string, int> SenIntoWords(std::string &s) {
    std::unordered_map<char, char> a;
    for (char i = 65;i < 91; ++i) {
        a[i] = i + 32;
    }
    std::string word;
    std::unordered_map<std::string, int> stoke;
    for (auto &item : s){
        if (isalpha(item)) {
            if (isupper(item)) {
                word += a[item];
            } else {
                word += item;
            }
        } else {
            if (!empty(word)) {
                stoke[word] += 1;
                word = "";
            }
        }
    }
    if (!empty(word)) {
        stoke[word]++;
    }
    return stoke;
};
std::vector<std::pair<std::string, const char*>> TextToSen(std::string_view &s) {
    std::string sen;
    std::vector<std::pair<std::string, const char*>> k;
    const char *p = s.begin();
    const char *f = s.begin();
    while (p != s.end()) {
        if (*p != '\n') {
            sen+=*p;
        } else {
            auto w = std::make_pair(sen,f);
            k.push_back(w);
            sen = "";
            if (p + 1 != s.end()) {
                f = p + 1;
            }
        }
        p++;
    }
    if (!empty(sen)) {
        auto w = std::make_pair(sen,f);
        k.push_back(w);
    }
    return k;
}
std::vector<std::string_view> Search(std::string_view text, std::string_view query, size_t results_count) {
    std::vector<std::unordered_map<std::string, int>> text_words;
    auto k = TextToSen(text);
    auto k1 = TextToSen(query);
    auto t = k1[0].first;
    auto s_w = SenIntoWords(t);
    std::vector<std::vector<Rational>> ans;

    for (auto i = 0; i < k.size(); ++i) {
        text_words.push_back(SenIntoWords(k[i].first));
        ans.push_back({0,i});
    }
    for (auto &item : s_w) {
        s_w[item.first] = 0;
    }
    for (auto &item : s_w){
        for (auto i = 0; i < text_words.size(); ++i) {
            if (text_words[i].contains(item.first)) {
                ++s_w[item.first];
            }

        }
        Rational p;
        if (s_w[item.first] != 0) {
            auto s1 = log(k.size()/s_w[item.first] );
            p = s1;
        }
        for (auto i = 0; i < text_words.size(); ++i) {
            if (text_words[i].contains(item.first)) {
                ans[i][0] += p*Rational(text_words[i][item.first], text_words[i].size());
            }
        }
    }
    std::sort(ans.begin(),ans.end());
    std::vector<std::string_view> ot;
    for (auto &item : ans)
        if (results_count > 0 && item[0] > 0) {
            std::string_view s2(k[item[1].GetNumerator()].second,k[item[1].GetNumerator()].first.size());
            ot.push_back(s2);
            results_count--;
        }

    std::reverse(std::begin(ot), std::end(ot));

    return ot;
}
int main (){
    long long p,v,q,m;
    std::string_view text =
            "Lorem Ipsum is simply dummy text\n"
            "of the printing and typesetting industry.\n"
            "Lorem Ipsum has been the industry's standard\n"
            "dummy text ever since the 1500s, when an unknown\n"
            "printer took a galley of type and scrambled it\n"
            "to make a type specimen book. It has survived\n"
            "not only five centuries, but also the leap into\n"
            "electronic typesetting, remaining essentially\n"
            "unchanged. It was popularised in the 1960s with\n"
            "the release of Letraset sheets containing Lorem\n"
            "psum passages, and more recently with desktop\n"
            "publishing software like Aldus PageMaker\n"
            "including versions of Lorem Ipsum.";


    std::string_view query = "typesetting release";
    auto k3 = Search(text, query, 3);
    std::cout<<k3[0]<<"\n"<<k3[1]<<"\n"<<k3[2];
};
```

```C++
#include "stack.h"
#include <iostream>

Node::Node(int32_t value, Node *previous) {
    Node::value = value;
    Node::previous = previous;
}
Stack::Stack() {
    head_ = nullptr;
    size_ = 0;
}
Stack::~Stack() {
    size_ = 0;
    while (head_ != nullptr) {
        Node *del_element = head_;
        head_ = del_element->previous;
        delete del_element;
    }
}
void Stack::Push(int32_t value) {
    Node *new_element_ptr = new Node(value, head_);
    head_ = new_element_ptr;
    ++size_;
}
void Stack::Pop() {
    if (head_ != nullptr) {
        Node *del_element = head_;
        head_ = del_element->previous;
        delete del_element;
        --size_;
    }
}
int32_t Stack::Top() const {
    return head_->value;
}
int32_t Stack::Size() const {
    return size_;
}
void Stack::Clear() {
    size_ = 0;
    while (head_ != nullptr) {
        Node *del_element = head_;
        head_ = del_element->previous;
        delete del_element;
    }
}
bool Stack::Empty() const {
    return head_ == nullptr;
}
```


```C++
#include <iostream>
#include <cmath>
#include "rational.h"
void Rational::Set(int64_t numer, int64_t denom) {
    int64_t nod = std::min(std::abs(numer), std::abs(denom));
    int64_t nod1 = std::max(std::abs(denom), std::abs(numer));
    while (nod != 0) {
        int64_t help = 0;
        help = nod;
        nod1 = nod1 % nod;
        nod = help;
        nod = nod1;
        nod1 = help;
    }
    denom_ = denom_ / std::abs(nod1);
    numer_ = numer_ / std::abs(nod1);
}
Rational::Rational() {
    numer_ = 0;
    denom_ = 1;
}
Rational::Rational(int64_t num, int64_t den) {
    if (den == 0) {
        throw RationalDivisionByZero{};
    }
    if (num != 0) {
        if (den >= 0) {
            numer_ = num;
            denom_ = den;
        } else {
            numer_ = -num;
            denom_ = -den;
        }
    } else {
        numer_ = num;
        denom_ = 1;
    }
    Set(numer_, denom_);
}
Rational::Rational(int64_t num) {
    numer_ = num;
    denom_ = 1;
}
int64_t Rational::GetNumerator() const {
    return numer_;
}
int64_t Rational::GetDenominator() const {
    return denom_;
}
void Rational::SetNumerator(int64_t value) {
    numer_ = value;
}
void Rational::SetDenominator(int64_t value) {
    if (value == 0) {
        throw RationalDivisionByZero{};
    }
    if (value < 0) {
        denom_ = -value;
        numer_ = -numer_;
    } else {
        denom_ = value;
    }
}
Rational& operator+=(Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.numer_ * rhs.denom_;
    int64_t b = lhs.denom_ * rhs.numer_;
    Rational ans(a + b, rhs.denom_ * lhs.denom_);
    lhs.numer_ = ans.numer_;
    lhs.denom_ = ans.denom_;
    return lhs;
}
Rational& operator*=(Rational& lhs, const Rational& rhs) {
    if (rhs.GetDenominator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    } else {
        int64_t a = lhs.numer_ * rhs.numer_;
        int64_t b = lhs.denom_ * rhs.denom_;
        Rational ans(a, b);
        lhs.numer_ = ans.numer_;
        lhs.denom_ = ans.denom_;
        return lhs;
    }
}
Rational& operator--(Rational& ratio) {
    Rational ans(ratio.numer_ - ratio.denom_, ratio.denom_);
    ratio.numer_ = ans.numer_;
    ratio.denom_ = ans.denom_;
    return ratio;
}
Rational& operator++(Rational& ratio) {
    Rational ans(ratio.numer_ + ratio.denom_, ratio.denom_);
    ratio.numer_ = ans.numer_;
    ratio.denom_ = ans.denom_;
    return ratio;
}
std::istream& operator>>(std::istream& is, Rational& ratio) {
    if (ratio.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    Rational ans(ratio.numer_, ratio.denom_);
    is >> ans.numer_ >> ans.denom_;
    return is;
}
Rational& operator/=(Rational& lhs, const Rational& rhs) {
    if (rhs.GetNumerator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    int sigh = 1;
    if (lhs.GetNumerator() * rhs.GetNumerator() < 0) {
        sigh = -1;
    }
    int64_t a = std::abs(lhs.GetNumerator()) * std::abs(rhs.GetDenominator()) * sigh;
    int64_t b = std::abs(lhs.GetDenominator()) * std::abs(rhs.GetNumerator());
    Rational ans(a, b);
    lhs.SetNumerator(ans.GetNumerator());
    lhs.SetDenominator(ans.GetDenominator());
    return lhs;
}
bool operator<(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a < b;
}
bool operator>(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a > b;
}
bool operator==(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a == b;
}
bool operator>=(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a >= b;
}
bool operator<=(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a <= b;
}
bool operator!=(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a != b;
}
Rational operator+(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    Rational ans(a + b, rhs.GetDenominator() * lhs.GetDenominator());
    return ans;
}
Rational operator-(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    Rational ans(a - b, rhs.GetDenominator() * lhs.GetDenominator());
    return ans;
}
Rational operator--(Rational& ratio, int) {
    Rational ans(ratio.GetNumerator() - ratio.GetDenominator(), ratio.GetDenominator());
    ratio.SetNumerator(ans.GetNumerator());
    ratio.SetDenominator(ans.GetDenominator());
    return ratio;
}
Rational operator++(Rational& ratio, int) {
    Rational ans(ratio.GetNumerator() + ratio.GetDenominator(), ratio.GetDenominator());
    ratio.SetNumerator(ans.GetNumerator());
    ratio.SetDenominator(ans.GetDenominator());
    return ratio;
}
Rational operator*(const Rational& lhs, const Rational& rhs) {
    if (rhs.GetDenominator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    int64_t a = lhs.GetNumerator() * rhs.GetNumerator();
    int64_t b = lhs.GetDenominator() * rhs.GetDenominator();
    Rational ans(a, b);
    return ans;
}
Rational operator/(const Rational& lhs, const Rational& rhs) {
    if (rhs.GetNumerator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    int sigh = 1;
    if (lhs.GetNumerator() * rhs.GetNumerator() < 0) {
        sigh = -1;
    }
    int64_t a = std::abs(lhs.GetNumerator()) * std::abs(rhs.GetDenominator()) * sigh;
    int64_t b = std::abs(lhs.GetDenominator()) * std::abs(rhs.GetNumerator());
    Rational ans(a, b);
    return ans;
}
std::ostream& operator<<(std::ostream& os, const Rational& ratio) {
    Rational ans(ratio.GetNumerator(), ratio.GetDenominator());
    if (ratio.GetDenominator() != 1 && ratio.GetNumerator() != 0) {
        os << ans.GetNumerator() << "/" << ans.GetDenominator();
    } else {
        os << ans.GetNumerator();
    }
    return os;
}
Rational operator+(const Rational& ratio) {
    Rational ans(ratio.GetNumerator(), ratio.GetDenominator());
    return ans;
}
Rational operator-(const Rational& ratio) {
    Rational ans(-1 * ratio.GetNumerator(), ratio.GetDenominator());
    return ans;
}
Rational& operator-=(Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    Rational ans(a - b, lhs.GetDenominator() * rhs.GetDenominator());
    lhs.SetNumerator(ans.GetNumerator());
    lhs.SetDenominator(ans.GetDenominator());
    return lhs;
}

```



```C++
#include <iostream>
#include <cmath>
#include "rational.h"

Rational::Rational() {
}
Rational::Rational(int64_t num, int64_t den) {
    if (num != 0) {
        numer_ = num;
        denom_ = den;
    } else {
        numer_ = num;
        denom_ = 1;
    }
    if (den == 0) {
        throw RationalDivisionByZero{};
    }
    Set(numer_, denom_);
}
Rational::Rational(int64_t num) {
    numer_ = num;
    denom_ = 1;
}
int64_t Rational::GetNumerator() const {
    return numer_;
}
int64_t Rational::GetDenominator() const {
    return std::abs(denom_);
}
void Rational::SetNumerator(int64_t value) {
    numer_ = value;
}
void Rational::SetDenominator(int64_t value) {
    denom_ = value;
}
void Rational::Set(int64_t numer, int64_t denom) {
    int64_t nod = std::min(std::abs(numer), std::abs(denom));
    int64_t nod1 = std::max(std::abs(denom), std::abs(numer));
    while (nod != 0) {
        int64_t help = 0;
        help = nod;
        nod1 = nod1 % nod;
        nod = help;
        nod = nod1;
        nod1 = help;
    }
    denom_ = denom_ / std::abs(nod1);
    numer_ = numer_ / std::abs(nod1);
}
Rational& operator+=(Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.numer_ * rhs.denom_;
    int64_t b = lhs.denom_ * rhs.numer_;
    Rational ans(a + b, rhs.denom_ * lhs.denom_);
    lhs.numer_ = ans.numer_;
    lhs.denom_ = ans.denom_;
    return lhs;
}
Rational& operator*=(Rational& lhs, const Rational& rhs) {
    if (rhs.GetDenominator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    } else {
        int64_t a = lhs.numer_ * rhs.numer_;
        int64_t b = lhs.denom_ * rhs.denom_;
        Rational ans(a, b);
        lhs.numer_ = ans.numer_;
        lhs.denom_ = ans.denom_;
        return lhs;
    }
}
Rational& operator--(Rational& ratio) {
    ratio.numer_ -= ratio.denom_;
    return ratio;
}
Rational& operator++(Rational& ratio) {
    ratio.numer_ += ratio.denom_;
    return ratio;
}
std::istream& operator>>(std::istream& is, Rational& ratio) {
    is >> ratio.numer_ >> ratio.denom_;
    return is;
}
Rational& operator/=(Rational& lhs, const Rational& rhs) {
    if (rhs.GetNumerator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    int sigh = 1;
    if (lhs.GetNumerator() * rhs.GetNumerator() < 0) {
        sigh = -1;
    }
    int64_t a = std::abs(lhs.GetNumerator()) * std::abs(rhs.GetDenominator()) * sigh;
    int64_t b = std::abs(lhs.GetDenominator()) * std::abs(rhs.GetNumerator());
    Rational ans(a, b);
    lhs.SetNumerator(ans.GetNumerator());
    lhs.SetDenominator(ans.GetDenominator());
    return lhs;
}
bool operator<(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a < b;
}
bool operator>(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a > b;
}
bool operator==(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a == b;
}
bool operator>=(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a >= b;
}
bool operator<=(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a <= b;
}
bool operator!=(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    return a != b;
}
Rational operator+(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    Rational ans(a + b, rhs.GetDenominator() * lhs.GetDenominator());
    return ans;
}
Rational operator-(const Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    Rational ans(a - b, rhs.GetDenominator() * lhs.GetDenominator());
    return ans;
}
Rational operator--(Rational& ratio, int) {
    ratio.SetNumerator(ratio.GetNumerator() - ratio.GetDenominator());
    return ratio;
}
Rational operator++(Rational& ratio, int) {
    ratio.SetNumerator(ratio.GetNumerator() + ratio.GetDenominator());
    return ratio;
}
Rational operator*(const Rational& lhs, const Rational& rhs) {
    if (rhs.GetDenominator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    int64_t a = lhs.GetNumerator() * rhs.GetNumerator();
    int64_t b = lhs.GetDenominator() * rhs.GetDenominator();
    Rational ans(a, b);
    return ans;
}
Rational operator/(const Rational& lhs, const Rational& rhs) {
    if (rhs.GetNumerator() == 0 || lhs.GetDenominator() == 0) {
        throw RationalDivisionByZero{};
    }
    int sigh = 1;
    if (lhs.GetNumerator() * rhs.GetNumerator() < 0) {
        sigh = -1;
    }
    int64_t a = std::abs(lhs.GetNumerator()) * std::abs(rhs.GetDenominator()) * sigh;
    int64_t b = std::abs(lhs.GetDenominator()) * std::abs(rhs.GetNumerator());
    Rational ans(a, b);
    return ans;
}
std::ostream& operator<<(std::ostream& os, const Rational& ratio) {
    if (ratio.GetDenominator() != 1 && ratio.GetNumerator() != 0) {
        os << ratio.GetNumerator() << "/" << ratio.GetDenominator();
    } else {
        os << ratio.GetNumerator();
    }
    return os;
}
Rational operator+(const Rational& ratio) {
    return ratio;
}
Rational operator-(const Rational& ratio) {
    Rational ans(-1 * ratio.GetNumerator(), ratio.GetDenominator());
    return ans;
}
Rational& operator-=(Rational& lhs, const Rational& rhs) {
    int64_t a = lhs.GetNumerator() * rhs.GetDenominator();
    int64_t b = lhs.GetDenominator() * rhs.GetNumerator();
    Rational ans(a - b, lhs.GetDenominator() * rhs.GetDenominator());
    lhs.SetNumerator(ans.GetNumerator());
    lhs.SetDenominator(ans.GetDenominator());
    return lhs;
}
```


```C++
#include <iostream>
#include <cmath>

class Rational {
private:
    int numer_;
    int denom_;
    void Set(int numer, int denom) {
        int nod = std::min(numer, denom);
        int nod1 = std::max(numer, denom);
        while (nod != 0) {
            int help;
            help = nod;
            nod1 = nod1 % nod;
            nod = help;
            nod = nod1;
            nod1 = help;
        }
        denom_ = denom_ / abs(nod1);
        numer_ = numer_ / abs(nod1);
    }
public:
    Rational() {

    }
    Rational(int  num, int den) {
        numer_ = num;
        denom_ = den;
    }
    Rational(int num) {
        numer_ = num;
        denom_ = 1;
    }
    int GetNumerator() const {
        return numer_;
    }
    int GetDenominator() const {
        return denom_;
    }
    void SetNumerator(int value) {
        numer_ = value;
    }
    void SetDenominator(int value) {
        denom_ = value;
    }
    friend Rational& operator+=(Rational& lhs, const Rational& rhs);
    friend std::ostream& operator<<(std::ostream& out, Rational& a);
    friend Rational operator/(const Rational& lhs, const Rational& rhs);
    friend Rational operator*(const Rational& lhs, const Rational& rhs);
    friend Rational operator++(Rational& ratio, int);
    friend Rational operator--(Rational& ratio, int);
    friend Rational operator+(const Rational& lhs, const Rational& rhs);
    friend Rational operator-(const Rational& lhs, const Rational& rhs);
    friend bool operator<(const Rational& lhs, const Rational& rhs);
    friend bool operator>(const Rational& lhs, const Rational& rhs);
    friend bool operator==(const Rational& lhs, const Rational& rhs);
    friend bool operator<=(const Rational& lhs, const Rational& rhs);
    friend bool operator>=(const Rational& lhs, const Rational& rhs);
    friend bool operator!=(const Rational& lhs, const Rational& rhs);
    friend std::istream& operator>>(std::istream& is, Rational& ratio);
    friend Rational& operator++(Rational& ratio);
    friend Rational& operator--(Rational& ratio);
    friend Rational& operator/=(Rational& lhs, const Rational& rhs);
    friend Rational& operator*=(Rational& lhs, const Rational& rhs);
};
Rational& operator*=(Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.numer_;
    int b = lhs.denom_ * rhs.denom_;
    Rational ans(a,b);
    ans.Set(ans.numer_,ans.denom_);
    lhs.numer_ = ans.numer_;
    lhs.denom_ = ans.denom_;
    return lhs;
}
Rational& operator/=(Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    Rational ans(a,b);
    ans.Set(ans.numer_,ans.denom_);
    lhs.numer_ = ans.numer_;
    lhs.denom_ = ans.denom_;
    return lhs;
}

Rational& operator--(Rational& ratio) {
    ratio.numer_ -= ratio.denom_;
    return ratio;
}
Rational& operator++(Rational& ratio) {
    ratio.numer_ += ratio.denom_;
    return ratio;
}
Rational& operator+=(Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    Rational ans(a + b, rhs.denom_ * lhs.denom_);
    ans.Set(ans.numer_,ans.denom_);
    lhs.numer_ = ans.numer_;
    lhs.denom_ = ans.denom_;
    return lhs;
}
bool operator<(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    return a < b;
}
bool operator>(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    return a > b;
}
bool operator==(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    return a == b;
}
bool operator>=(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    return a >= b;
}
bool operator<=(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    return a <= b;
}
bool operator!=(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    return a != b;
}
Rational operator+(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    Rational ans(a + b, rhs.denom_ * lhs.denom_);
    ans.Set(ans.numer_,ans.denom_);
    return ans;
}
Rational operator-(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    Rational ans(a - b, rhs.denom_ * lhs.denom_);
    ans.Set(ans.numer_,ans.denom_);
    return ans;
}
Rational operator--(Rational& ratio, int) {
    ratio.numer_ -= ratio.denom_;
    return ratio;
}
Rational operator++(Rational& ratio, int)  {
    ratio.numer_ += ratio.denom_;
    return ratio;
}
Rational operator*(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.numer_;
    int b = lhs.denom_ * rhs.denom_;
    Rational ans(a,b);
    ans.Set(ans.numer_,ans.denom_);
    return ans;
}
Rational operator/(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    Rational ans(a,b);
    ans.Set(ans.numer_,ans.denom_);
    return ans;
}
std::ostream& operator<<(std::ostream& os, Rational& a) {
    a.Set(a.numer_,a.denom_);
    os << a.numer_ << "/" << a.denom_;
    return os;
}
std::istream& operator>>(std::istream& is, Rational& ratio) {
    is>>ratio.numer_>>ratio.denom_;
    return is;
}
int main() {

    Rational a = Rational(12, 4);
    Rational b = Rational(56);
    b/=a;
    a++;
    ++a;
    a+=b;
    b=a-b+a-b+2*b;
    std::cout <<b;
    Rational c;
    std::cin>>c;

}
```









































#include <iostream>
#include <cmath>

class Rational {
private:
    int numer_;
    int denom_;
    void Set(int numer, int denom) {
        int nod = std::min(numer, denom);
        int nod1 = std::max(numer, denom);
        while (nod != 0) {
            int help;
            help = nod;
            nod1 = nod1 % nod;
            nod = help;
            nod = nod1;
            nod1 = help;
        }
        denom_ = denom_ / abs(nod1);
        numer_ = numer_ / abs(nod1);
    }
public:
    Rational()=default;
    Rational(int  num, int den) {
        numer_ = num;
        denom_ = den;
    }
    Rational(int num) {
        numer_ = num;
        denom_ = 1;
    }
    int GetNumerator() const {
        return numer_;
    }
    int GetDenominator() const {
        return denom_;
    }
    void SetNumerator(int value) {
        numer_ = value;
    }
    void SetDenominator(int value) {
        denom_ = value;
    }
    friend Rational& operator+=(Rational& lhs, const Rational& rhs);
    friend std::ostream& operator<<(std::ostream& out, Rational& a);
    friend Rational operator/(const Rational& lhs, const Rational& rhs);
    friend Rational operator*(const Rational& lhs, const Rational& rhs);
    friend Rational operator++(Rational& ratio, int);
    friend Rational operator--(Rational& ratio, int);
    friend Rational operator+(const Rational& lhs, const Rational& rhs);
    friend Rational operator-(const Rational& lhs, const Rational& rhs);
    friend bool operator<(const Rational& lhs, const Rational& rhs);
    friend bool operator>(const Rational& lhs, const Rational& rhs);
    friend bool operator==(const Rational& lhs, const Rational& rhs);
    friend bool operator<=(const Rational& lhs, const Rational& rhs);
    friend bool operator>=(const Rational& lhs, const Rational& rhs);
    friend bool operator!=(const Rational& lhs, const Rational& rhs);
    friend std::ostream& operator<<(std::ostream& os, const Rational& ratio);
    friend Rational& operator++(Rational& ratio);
    friend Rational& operator--(Rational& ratio);
    friend Rational& operator/=(Rational& lhs, const Rational& rhs);
    friend Rational& operator*=(Rational& lhs, const Rational& rhs);
};
Rational& operator*=(Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.numer_;
    int b = lhs.denom_ * rhs.denom_;
    Rational ans(a,b);
    ans.Set(ans.numer_,ans.denom_);
    lhs.numer_ = ans.numer_;
    lhs.denom_ = ans.denom_;
    return lhs;
}
Rational& operator/=(Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    Rational ans(a,b);
    ans.Set(ans.numer_,ans.denom_);
    lhs.numer_ = ans.numer_;
    lhs.denom_ = ans.denom_;
    return lhs;
}

Rational& operator--(Rational& ratio) {
    ratio.numer_ -= ratio.denom_;
    return ratio;
}
Rational& operator++(Rational& ratio) {
    ratio.numer_ += ratio.denom_;
    return ratio;
}
Rational& operator+=(Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    Rational ans(a + b, rhs.denom_ * lhs.denom_);
    ans.Set(ans.numer_,ans.denom_);
    lhs.numer_ = ans.numer_;
    lhs.denom_ = ans.denom_;
    return lhs;
}
bool operator<(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    return a < b;
}
bool operator>(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    return a > b;
}
bool operator==(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    return a == b;
}
bool operator>=(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    return a >= b;
}
bool operator<=(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    return a <= b;
}
bool operator!=(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    return a != b;
}
Rational operator+(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    Rational ans(a + b, rhs.denom_ * lhs.denom_);
    ans.Set(ans.numer_,ans.denom_);
    return ans;
}
Rational operator-(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    Rational ans(a - b, rhs.denom_ * lhs.denom_);
    ans.Set(ans.numer_,ans.denom_);
    return ans;
}
Rational operator--(Rational& ratio, int) {
    ratio.numer_ -= ratio.denom_;
    return ratio;
}
Rational operator++(Rational& ratio, int)  {
    ratio.numer_ += ratio.denom_;
    return ratio;
}
Rational operator*(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.numer_;
    int b = lhs.denom_ * rhs.denom_;
    Rational ans(a,b);
    ans.Set(ans.numer_,ans.denom_);
    return ans;
}
Rational operator/(const Rational& lhs, const Rational& rhs) {
    int a = lhs.numer_ * rhs.denom_;
    int b = lhs.denom_ * rhs.numer_;
    Rational ans(a,b);
    ans.Set(ans.numer_,ans.denom_);
    return ans;
}
std::ostream& operator<<(std::ostream& out, Rational& a) {
    a.Set(a.numer_,a.denom_);
    if (a.denom_ !=1) {
        out << a.numer_ << "/" << a.denom_;
    } else {
        out << a.numer_;
    }
    return out;
}
std::ostream& operator<<(std::ostream& os, const Rational& ratio) {
    os<<ratio.numer_<<ratio.denom_;
    return os;
}
int main() {
    Rational a = Rational(10323232, 132325);
    Rational b = Rational(205454);
    Rational c=a/b;

    std::cout << c<<" "<<a;




}

<h3>С++ 3 модуль лекции</h3>
<details><summary>Лекция 2</summary>

   *1. Язык С++ компилируемый, код в нем сразу превращаеться в испольняемые файлы, которые напрямую поступают к процессору.*
    
   *2. Компиляторы превращают код в понятный для компьютера язык(два самых популярных: clang++ и g++)*
    
   *3. Программа сначало запускает функция main(), потом все отсальное.*
    
   ```C++
   int main {
    
   }
   ```
   **Вывод:** 
    
   ![](https://github.com/Kera-0/test/blob/main/%D0%A4%D0%AB%D0%A4%D0%AB%D0%A4%D0%AB%D0%92%D0%AB.PNG)
   ```C++
   int foo {
    
   }
   ```
   **Вывод:** 
    
   ![](https://github.com/Kera-0/test/blob/main/%D0%BE%D1%88%D0%B8%D0%B1%D0%BA%D0%B0.PNG)
   ![](https://github.com/Kera-0/test/blob/main/%D1%86%D0%B9%D1%86%D0%B9%D1%86%D0%B9.PNG)
    
   *4. int пишем перед main т.к она возращает число.*
    
    
    
   *5. возращает одно и тоже*
   ```C++
   int main () {
      return 0;
   }
   ```
   ```C++
   int main () {
        
   }
   ```
   *6. Через return, повилось понимать, что  программа завершилась успешно,елси она возращает 0 код, и другое в ином случае.*
    
   *7. `#include <iostrem>` - для доступа к механизмам ввода (как import в питоне)*
    
   * 1 `std::cin >>` ввод объекта
   * 2 `std::cout <<` вывод объекта 
   * 3 `<< "/n";` или ` << std::endl;` - перенос на новую строку
   * 4 все переменные надо определять на старет, переменная не может поменять свой тип в С++ 
   * 5 int знаковое число(`int a;`)
   *6 `''` - используеться для вывода одного символа `"для ввода стрки"`, иначе будте ошибка(`"ф"` - это не одна буква т.к `''` поддерживает только ASCII) 

  *8) `if` должен быть с `()` и из-за отсутсвия `{}` могут быть ошибки*

  **Пример:**
```C++
#include <iostrem>
int main() {
    int a;
    std::cin >> a;
    if (a % 2 == 0) {
        std::cout << a << "even\n";
    } else {
        std::cout << a << "odd\n";
    }
}

```
*9) bool отвечают только за `true` и `false`*

10) Разница `a++` и `++a`;
```C++
#include <iostrem>
int main() {
    int a = 123;
    std::cout << a++;    
}
```

**Вывод: 123** 

```C++
#include <iostrem>
int main() {
    int a = 123;
    std::cout << ++a;    
}
```

**Вывод: 124** 

11) цикл бесконечный
```
#include <iostrem>
int main() {
    for (;;) {
        std::cout << "1 ";
    }
}
```

**Вывод: 1 1 1 ... 1 1 1 ...

    
12) Если мы хотим чтобы переменные были доступны только в определенном блоке, то
```
#include <iostrem>
int main {


    {
        int a = 5;
    }
    {
        int a;
        std::cout << a;
    }
}
```
**Вывод: число из неинициализированная памяти т.к это две разные переменные а**

13) с начало выпольняеться один раз потом проверяет условие на while
```
do {

} while (true);

```
14) switch в a передаем переменную,она сравниваеться с case, в switch нельзя создавать переменную и если не написать break, то прогрмма выведет все начиная с подходящего case
![](https://github.com/Kera-0/-lecture-notes/blob/main/awqwq.PNG)
15) `a << c` это мы число a умножаем на 2 в степени с
16) `&&` `||` - булевый оператор. 
17) `&` `|` - битовый оператор.



















</details>

<details><summary>Лекция 3</summary>
  
  ![](https://github.com/Kera-0/-lecture-notes/blob/main/32312.PNG)

  0) в С++
  
  ![](https://github.com/Kera-0/-lecture-notes/blob/main/git%201.PNG)
  
  1) `auto` - автоматическое определенние типа

  ![](https://github.com/Kera-0/-lecture-notes/blob/main/git%202.PNG)
  
  2) нельзя `void n = 5`
  
  ![](https://github.com/Kera-0/-lecture-notes/blob/main/git%203.PNG)
  
  3) байт - минимальная адресуемая единица памяти
  
  4) `signed` - число со знаком, `unsigned` - без знаковое число
  
  5) используем `int64_t` т.к в зависимости от системы `long,long long, int` вмещают разное количестов памяти.

  5) ` int x = (1u << 31) - 1 ` 1u => это короткая запись безнаковой единицы(это штука = 2^31-1, используем беззнаковую, чтобы не было переполнения )  (`unsigned int x = 1`)
     
  7) `std::bitset<8>(i)` переводит i в двоичную запись, 8 отвечает за количество знаков в числе(i = 2 -> 00000010)
  8) чтобы из `i` получить `-i`, нужно `~(i - 1)`, `~` битовое отрицание 0->1 1->0
  9) вещественные числа
  
  ![](https://github.com/Kera-0/-lecture-notes/blob/main/%D0%BF%D1%88%D0%B5%205.PNG)

   fraction отвечает за числа после запятой
   
   12) вывод вещественного числа 
  `std::cout << std::fixed << std::precision(i) << x; ` вывод числа x с i знаками после запятой









  
</details>
<details><summary>Лекция 4</summary>

  1) `size_t` хранит размер числа (храниться в `#include <cstdint>`).
  2) циклы
```
  C++
    size_t size;
    for (size_t i = size; ~i; --i) {

    }
```



Это компактный способ проитерироваться от конца до начала.(`size_t` беззнаковое число => мы всместо `~i` не можем писать `i >= 0` )
    
3) Функции, если функция ничего не возращает, то тип `void`
```C++
        void foo() {

        }
```

Если она что-то возращает,то нужно писать тип который она возращает перед функцией.

```C++
        int foo(int a, int b) {
            return a + b
        }
```
4) Писать функции нужно до `int main() {}`, однако функции можно объявить до программы, но ее функционал написать после





 ![](https://github.com/Kera-0/-lecture-notes/blob/main/git%206.PNG)

5) Передача аргументов в функцию(копия)


 ![](https://github.com/Kera-0/-lecture-notes/blob/main/git%207.PNG)

**Вывод:**

**Hello, Bob!**
        
  **world**
  
Функция создает, копию аргумента name(она остаеться в внутри функции и не меняеться в main) это плохо т.к мы тратим 2 раза больше памяти и тратим ресурсы процессора на копирование

6)Чтобы не крпировать нужно использовать `&`, однако внутри функции ее можно будет поменять

![](https://github.com/Kera-0/-lecture-notes/blob/main/git%208.PNG)

**Вывод: **

**Hello, Bob!**
        
  **Bob**
  
7) Чтобы нельзя было менять нужно писать `const std::string& name`

  

8) чтобы отбросить константность нужно

```C++
const std::string name = "a";
std::string& ref = const_cast<std::string&>(name);
```
теперь `ref` это тоже самое, что `name`, только `ref` можно менять(name тоже поменяеться)

9) Указатель ` std::string* ptr = &bob`  `&bob` - это адрес переменной(это номер байтика в памяти с которого начинаеться `bob`),`std::string* ptr` - указатель на строчку
    
     1)Указатель можно менять\переприсваивать
   
     2)если мы его используем то перед всеми аргументами нужно ставить `*` пример: `*ref = *a + 1 + *b `

     3)чтобы передать переменную в функцию вида `void dup(std::string* ref)` нужно делать так `dup(&ref)`

10) можно использовать одну функцию для разных типов данных программа будет понимать,что использовать по элементам которые передаються  в функцию

![](https://github.com/Kera-0/-lecture-notes/blob/main/git%209.PNG)


</details>

<details><summary>Лекция 5</summary>
  
1) `int name[10] = {} ` создать массив из 10 элементов и заполнить его нулями

2) `sizeof(k)` найти размер элемента\структуры в байтах

3) найти размер массива `sizeof(arr) / sizeof(arr)[0]` или `std::size(arr)`

4) чтобы передать массив ссылкой в функцию нужно `void foo (const int (&arr)[размер])`

5) чтобы использовать удобный синтаксис можно vector

![](https://github.com/Kera-0/-lecture-notes/blob/main/git%2010.PNG)   

6) Передать массив c произвольным размером

![](https://github.com/Kera-0/-lecture-notes/blob/main/git%2011.PNG)  

это работает т.к `arr[i]` это тоже самое, что и `*(arr + i)` это есть мы двигаем указатель на i*sizeof(i) байт 

7) вывод массива

   ![](https://github.com/Kera-0/-lecture-notes/blob/main/git%2012.PNG)

8) сортировка массива `std::sort(&arr[0], &arr[0] + std::size(arr))` первый указатель на начало массива второй на элемент **после** последнего
9) `const int* begin` это читаеться как указатель на константный инт, то есть ` begin` не константа его можно менять, но на то куда указывает `begin` это константа
10) Сайт для перевода с языка [ С++ на английский ]( https://cdecl.org/ )

    

    
12) массив представлен, как подряд идущие элементы в памяти
13) если из одной стуктуры данных хотим ее как-то превратить в дургую, то используем `static_cast\reinterpret_cast`
    
a = '10'

например `static_cast<int>(a)` превратит строку a в число 10 так нельзя `(int)a`
14) `std::swap(a,b)` меняет элементы а и b местами
15) нельзя использовать указатели в двумерных массивах
</details>
<details><summary>Лекция 6</summary>

1) строчка в с++ - это набор байт

```C++
  int main() {
  char str[] = {'H', 'e'};
  }
```
2) компактный вывод строки


![](https://github.com/Kera-0/-lecture-notes/blob/main/git%2013.PNG)

это работает т.к каждая строка в С++ заканчиваеться на `\0`

3) `strlen(i)` - длина строки i
4) `strcmp(i,i1)` - функция стравнивает две строки лексиграфически ввыводит 1 `i > i1` 0 `i = i1` -1 `i < i1`
5) структуры объект данных,который может хранить разные типы.

   
**Пример:**
```C++
struct Cstring {
    const char* begin; 
    size_t size;

};
int Out(const Cstring& str) {
    for (size_t i = 0;i < str.size; ++i) { 
        std::cout << str.begin[i];
    }
}
int main() {
    Cstring k; # или Cstring k {"aascxcx", 7}
    k.begin = "aascxcx";
    k.size = strlen("aascxcx");
    Out(k);
}
```
**Вывод:**
`aascxcx`

6) `const char* ptr = "abcd"` указатель  на первый элемент строчки
</details>

