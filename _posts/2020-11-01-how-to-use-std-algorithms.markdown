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
    {
        // Mismatch
        const std::array a1 = {1, 2, 3, 4, 5};
        const std::array a2 = {1, 2, 4, 4, 5};
    
        const auto m = std::mismatch(std::cbegin(a1), std::cend(a1), std::cbegin(a2)); 
        std::cout << "In arrays:" << "\n";
        std::cout << "{";
        for (const auto v : a1) {
            std::cout << v << ", ";
        }
        std::cout << "}\n";
        std::cout << "{";
        for (const auto v : a2) {
            std::cout << v << ", ";
        }
        std::cout << "}\n";
        std::cout << "mismatches are: ";
        std::cout << *m.first << ", " << *m.second << "\n";
    }

    {
        // Partial sum
        std::vector<int> data_set = {2, 2, 2, 2, 2, 2, 2, 2, 2, 2}; // or std::vector<int>v(10, 2);
        std::cout << "All following examples done on the array: {";
        for (const auto value : data_set) {
            std::cout << value << ", ";
        }

        using printInt = std::ostream_iterator<int>;
        std::cout << "The first 10 even numbers are: ";
        std::partial_sum(data_set.begin(), data_set.end(), printInt(std::cout, " "));
        std::cout << '\n';

        std::cout << "The first 10 with minus operation: ";
        std::partial_sum(data_set.begin(), data_set.end(), printInt(std::cout, " "), std::minus<int>{});
        std::cout << '\n';

        std::cout << "The first 10 powers of 2 are: ";
        std::partial_sum(data_set.begin(), data_set.end(), printInt(std::cout, " "), std::multiplies{});
        std::cout << '\n';

        // Inclusive scan
        std::cout << "The inclusive scan: ";
        std::inclusive_scan(data_set.begin(), data_set.end(), printInt(std::cout, " "));
        std::cout << '\n';

        // Exclusive scan
        std::cout << "The exclusive scan: ";
        std::exclusive_scan(data_set.begin(), data_set.end(), printInt(std::cout, " "), 0);
        std::cout << '\n';
    }

    {
        // Accumulate
        std::vector data_set = {1, 2, 3, 4};
        std::cout << "All following examples done on the array: {";
        for (const auto value : data_set) {
            std::cout << value << ", ";
        }
        std::cout << "}\n";

        const auto acc = std::accumulate(std::cbegin(data_set), std::cend(data_set), 1, std::multiplies{});
        std::cout << "Multiplication of";
        for (const auto value: data_set) {
            std::cout << " " << value << ",";
        }
        std::cout << " is " << acc << "\n";

        // Transform reduce
        const auto y = std::transform_reduce(std::cbegin(data_set), std::cend(data_set), std::cbegin(data_set), 0);
        const auto z = std::transform_reduce(std::cbegin(data_set), std::cend(data_set), std::cbegin(data_set), 0,
                                             std::plus{}, std::multiplies{});
        std::cout << "Default transform reduce is " << "\n";
        std::cout << y << "\n";
        std::cout << z << "\n";
    }
}


{% endhighlight %}

[See it on Compiler Explorer][compiler-explorer]

[compiler-explorer]: https://godbolt.org/z/M5vT6M3GK
