---
layout: post
title:  "How to use standard algorithms in C++"
date:   2020-11-01 15:02:48 +0100
categories: c++ developpement
---

{% highlight C++ %}
#include <algorithm>
#include <functional>
#include <iostream>
#include <iterator>
#include <numeric>
#include <vector>

int main() {
    const std::array a1 = {1, 2, 3, 4, 5};
    const std::array a2 = {1, 2, 4, 4, 5};

    // Mismatch
    const auto m = std::mismatch(std::cbegin(a1), std::cend(a1), std::cbegin(a2)); 
    std::cout << *m.first << ", " << *m.second << "\n";

    // Partial sum
    std::vector<int> v = {2, 2, 2, 2, 2, 2, 2, 2, 2, 2}; // or std::vector<int>v(10, 2);
    using printInt = std::ostream_iterator<int>;
    std::cout << "Partial sum: ";
    std::partial_sum(v.begin(), v.end(), printInt(std::cout, " "));
    std::cout << '\n';

    std::cout << "The first 10 even numbers are: ";
    std::partial_sum(v.begin(), v.end(), printInt(std::cout, " "), std::minus<int>{});
    std::cout << '\n';

    std::cout << "The first 10 powers of 2 are: ";
    std::partial_sum(v.begin(), v.end(), printInt(std::cout, " "), std::multiplies{});
    std::cout << '\n';

    // Inclusive scan
    std::cout << "The inclusive scan: ";
    std::inclusive_scan(v.begin(), v.end(), printInt(std::cout, " "));
    std::cout << '\n';

    // Exclusive scan
    std::cout << "The exclusive scan: ";
    std::exclusive_scan(v.begin(), v.end(), printInt(std::cout, " "), 0);
    std::cout << '\n';

    // Accumulate
    std::vector v2 = {1, 2, 3, 4};
    const auto acc = std::accumulate(std::cbegin(v2), std::cend(v2), 1, std::multiplies{});
    std::cout << "Multiplication of";
    for (const auto value: v2) {
        std::cout << " " << value << ",";
    }
    std::cout << " is " << acc << "\n";

    // Transform reduce
    const auto y = std::transform_reduce(std::cbegin(v2), std::cend(v2), 0,
                                         std::plus{},
                                         [](auto e) { return e * e;});
    std::cout << y << "\n";
}
{% endhighlight %}

