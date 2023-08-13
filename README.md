# cpp_slice_pattern

```c++
template <typename C>
class Slice {
    public:
    using iterator = typename C::iterator;
    using value_type = typename C::value_type;
    explicit Slice(iterator begin_, iterator end_) : begin_(begin_), end_(end_) {};
    iterator begin_;
    iterator end_;
    constexpr auto begin() { return begin_; }
    constexpr auto end() { return end_; }
};

 template<typename Container>
 auto slice( Container && c, int from=0, int to=0) 
 {
    auto it_from = (from < 0) ? c.end() + from : c.begin() + from;
    auto it_to = (to <= 0) ?  c.end() + to : c.begin() + to;
    if (std::distance(it_from, it_to) < 0 )
        std::swap(it_from, it_to);

    return Slice<std::decay_t<Container>>(it_from ,it_to);
 }
```
