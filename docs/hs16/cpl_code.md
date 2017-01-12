<!-- For Print -->
# C++ Übungen
## Count Characters (with whitespace)
```c++
int allcharcount(std::istream &in){
   using it=std::istreambuf_iterator<char>;
   return std::distance(it{in}, it{});
}
```

## Word List
Reads all words from stream and outputs a set, ignoring case
```c++
std::set<std::string> wlist(std::istream &in) {
	using input = std::istream_iterator<std::string>;
	input in_iterator { in };
	input eof { };

	std::vector<std::string> allwords { in_iterator, eof };
	std::transform(allwords.begin(), allwords.end(), allwords.begin(),
			[](std::string x) {
                transform(x.begin(),x.end(),x.begin(), ::tolower); return x;
            });

	std::set<std::string> uniqueWords { allwords.begin(), allwords.end() };

	return uniqueWords;
}

void printSet(std::set<std::string> set, std::ostream &out) {
	copy(set.begin(), set.end(), std::ostream_iterator<std::string> { out, "\n" });
}
```

## Algorithms
### Remove certain elements from container
```c++
std::vector<int> myInts{1, 2, 3, 4};
// remove() shifts the to be removed elements
// to the end and returns an iterator to the new end
// of the container
myInts.erase(
    std::remove(myInts.begin(), myInts.end(), 3), 
    myInts.end());
```

```c++
auto it_remove = std::remove_if(myInts.begin(), myInts.end(),
	    [](int i) { return i % 2 == 0; });
myInts.erase(it_remove, myInts.end());
// myInts = {1, 3}
```

### lexicographical_compare
```c++
bool lexi_compare()(string const &l, string const &r){
    return std::lexicographical_compare(
        l.begin(), l.end(), r.begin(), r.end(),
        [](char l, char r){
            return std::tolower(l) < std::tolower(r);
        });
}
```

## Iterators
### Print vector
```c++
void print(std::vector<int> const & v, std::ostream & out) {
	std::ostream_iterator<int> os{out, ", "};
	std::copy(v.begin(), v.end(), os);
}
```

## Functions with the Word Class
### Count words in stream
```c++
unsigned wcount(std::istream& in) {
    return std::distance(std::istream_iterator<Word>(in), std::istream_iterator<Word>());
}
```
### Count different words in stream
```c++
unsigned wdiffcount(std::istream& in) {
    std::set<Word> words{std::istream_iterator<Word>(in), std::istream_iterator<Word>()};
    return words.size();
}
```

### Print 20 most common words from input stream
```c++
void wfavorite(std::istream & in, std::ostream & out) {
	std::map<word::Word, unsigned> map { };
	word::Word w { };
	while (in >> w) {
		++map[w];
	}
	std::vector<std::pair<word::Word, unsigned>> v { };
	std::copy(map.begin(), map.end(), std::back_inserter(v));
	std::sort(v.begin(), v.end(), [](auto a, auto b) {return a.second > b.second;});
	int count{};
	count = v.size() < 20 ? v.size() : 20;
	std::copy_n(v.begin(), count, std::ostream_iterator<std::pair<word::Word, unsigned>>(out, ""));
}
```

## is_prime (ugly)
```c++
bool is_prime(unsigned long long n) {
	if (n < 2)
		return false;
	if (n == 2)
		return true;
	if (n % 2 == 0)
		return false;
// can you use an algorithm here as well?
	for (int i = 3; (i * i) <= n; i += 2) {
		if (n % i == 0)
			return false;
	}
	return true;

}
```

## Type Conversion Operator
```c++
struct To {
    To() = default;
    To(const struct From&) {} // converting constructor
};
 
struct From {
    operator To() const {return To();} // conversion function
};
```

## Rekursive Datenstruktur (shared_ptr)
Every module has a name, a list of requirements (successors) and back-references to its own predecessors.

To resolve cyclic dependencies, `weak_ptr` are used for the predecessors (back-references)
```c++
struct module;

using module_ptr = std::shared_ptr<module>;
using modules = std::vector<module_ptr>;
using module_catalog = std::map<std::string, module_ptr>;

struct module : std::enable_shared_from_this<module> {
    std::string const name;
    
    // declare explicit to avoid implicit type conversion
	explicit module(std::string const & name) : name {name}{};

	void add_predecessor(module_ptr predecessor) {
        // use weak pointers for back-refs (predecessors)
		std::weak_ptr<module> wp{predecessor}; 
		_predecessors.push_back(wp);
		predecessor->_successors.push_back(shared_from_this());
	}

	bool has_predecessor() const {
		return !_predecessors.empty();
	}

	void update_earliest_semester(unsigned newEarliestSemester) {
		earliest_semester = std::max(newEarliestSemester, earliest_semester);
	}

	unsigned get_earliest_semester() const {
		return earliest_semester;
	}

	modules successors() const {
		return _successors;
	}

	modules predecessors() {
        // clean up expired pointers, to make sure every share_pointer is valid
		remove_expired_predecessors();
		modules predecessors_shared{_predecessors.size()};
        // convert weak_pointers to "real" shared_pointers
		std::transform(std::begin(_predecessors), std::end(_predecessors),
            std::begin(predecessors_shared), [](module_weak_ptr m) {
			    return m.lock();
		});
		return predecessors_shared;
	}

	
private:
	using module_weak_ptr = std::weak_ptr<module>;
	using module_back_references = std::vector<module_weak_ptr>;

	void remove_expired_predecessors() {
        // clean up expired weak_ptrs
		auto removed_begin = std::remove_if(std::begin(_predecessors), 
            std::end(_predecessors), [](module_weak_ptr m) {
			    return m.expired();
		    });
		_predecessors.erase(removed_begin, std::end(_predecessors));
	}

	unsigned earliest_semester{0};
	module_back_references _predecessors{};
	modules _successors{};

};
```
### Read file and create modules
```c++
namespace {

module_ptr get_module_ptr(std::string const & module_name, module_catalog & catalog) {
	if (catalog.count(module_name) == 0) {
		auto m = std::make_shared<module>(module_name);
		catalog[module_name] = m;
	}
	return catalog[module_name];
}

void add(module_catalog& catalog, std::string const & line) {
	std::stringstream line_stream{line};
	std::string module_name;
	line_stream >> module_name;
	auto module = get_module_ptr(module_name, catalog);
	std::for_each(std::istream_iterator<std::string>{line_stream},
        std::istream_iterator<std::string>{}, 
        [&](std::string const & module_name){
		    auto dependency = get_module_ptr(module_name, catalog);
		    module->add_predecessor(dependency);
	    });
}
}

module_catalog read_modules(std::istream & input) {
	module_catalog catalog{};
	std::string line;
	while (getline(input, line)) {
		add(catalog, line);
	}
	return catalog;
}
```
