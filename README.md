# cpp_slice_pattern

## ver 0
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

## ver 1
```c++
template <typename iterator>
class Slice {
    private:
    iterator begin_;
    iterator end_;
    public:
    using value_type = decltype(*(iterator{}));    
    using difference_type = decltype(std::distance(iterator{},iterator{}));
    explicit Slice(iterator begin_, iterator end_) : begin_(begin_), end_(end_) {};
    template <typename container>
    explicit Slice(container && c) : Slice(c.begin(), c.end()) {};
    constexpr auto begin() const noexcept { return begin_; }
    constexpr auto end() const noexcept { return end_; }
    constexpr auto slice( difference_type from=0, difference_type to=0) const noexcept {
        auto it_from = (from < 0) ? end_ + from : begin_ + from;
        auto it_to = (to <= 0) ?  end_ + to : begin_ + to;
        if (std::distance(it_from, it_to) < 0 )
            std::swap(it_from, it_to);
        return Slice(it_from,it_to);
    }
  constexpr size_t size() const noexcept {
        return std::distance(begin_, end_);
    }

    constexpr bool empty() const noexcept {
        return begin_ == end_;
    }    
};

template<typename T>
Slice(T) -> Slice<decltype(T{}.begin())>;
```
