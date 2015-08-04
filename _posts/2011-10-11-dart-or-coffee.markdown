---
layout: post
title: "Dart or Coffeescript"
date: 2011-10-11 10:20
comments: true
categories:
- coffeescript
- javascript
---

![Dart or Coffeescript](/images/dart_or_coffee.png)

There has been a lot of buzz lately regarding the emergence of a new web programming language called Dart from Google. Dart, like [Coffeescript](http://coffeescript.org/),
is a language that is interpreted into javascript and allows you to code in a more object oriented style when developing the front-end of your 
web applications. A few people have asked if they should use Coffeescript of Dart. To that, I say to learn javascript, use coffeescript, and check out Dart. 
Both languages attempt to extract the beautiful object model hidden beneath all the muck in javascript. Both languages ([especially coffeescript](http://coffeescript.org/#classes) 
do this but Dart carries around a lot of dead weight. While Dart is pretty neat and backed by Google, it has not be absorbed into the web-applications development community like coffeescript has 
(coffeescript is the suggested front-end development language of the Rails core team and the preferred front-end language of the ruby community).
Coffeescript also has wider support tools and, in my personal opinion, more readability. However, if that does not have you convinced then you can take
a look at this 'Hello World' program written in Coffeescript and compiled to Javascript then Dart compiled to Javascript (taken from [this gist](https://gist.github.com/1277224)).

<!--more-->

Long story short, Dart offers enhanced security in your front-end and is more catered to large-scale projects 
but I won't be reaching for Dart in the near future. The language syntax is cool and liken to Java but all the useless generated code really makes it
hard for me to understand what is going on under the hood. Why use 17K lines of code when 11 will do?

###First Coffeescript

``` coffeescript
# Copyright (c) 2011, the <your_company> project authors. Please don't see the
# AUTHORS file for details. All rights reserved. Use of this source code is
# governed by a BSD-style license that can be found in the LICENSE file.
# Simple test program invoked with an option to eagerly
# compile all code that is loaded in the isolate.
# VMOptions=--fuu_dart

class HelloCoffeeTest
    test_main: ->
        console.log "Hello, Coffee man!"

new HelloCoffeeTest().test_main()
```

``` javascript
(function() {
  var HelloCoffeeTest;
  HelloCoffeeTest = (function() {
    function HelloCoffeeTest() {}
    HelloCoffeeTest.prototype.test_main = function() {
      return console.log("Hello, Coffee man!");
    };
    return HelloCoffeeTest;
  })();
  new HelloCoffeeTest().test_main();
}).call(this);
```

###Now Dart

```java
// Copyright (c) 2011, the Dart project authors.  Please see the AUTHORS file
// for details. All rights reserved. Use of this source code is governed by a
// BSD-style license that can be found in the LICENSE file.
// Simple test program invoked with an option to eagerly
// compile all code that is loaded in the isolate.
// VMOptions=--compile_all

class HelloDartTest {
  static testMain() {
    print("Hello, Darter!");
  }
}


main() {
  HelloDartTest.testMain();
}

/**
 * @param {!RTT} other
 * @return {boolean} Whether this type is implement by other
 */
RTT.prototype.implementedByType = function(otherType) { 
  if (otherType === this || otherType === RTT.dynamicType) {
    return true;
  }
  var targetTypeInfo = $mapLookup(otherType.implementedTypes, this.classKey);
  if (targetTypeInfo == null) { 
    return false; 
  }
  if (targetTypeInfo.typeArgs && this.typeArgs) {
    for(var i = this.typeArgs.length - 1; i >= 0; i--) {
      if (!this.typeArgs[i].implementedByType(targetTypeInfo.typeArgs[i])) {
        return false;
      }
    }
  }
  return true;
};

/**
 * @return {string} the class name associated with this type
 */
RTT.prototype.getClassName = function() {
  var name = this.classKey;
  if (name.substr(0, 4) == "cls:") {
    name = name.substr(4);
  }
  if (name.substr(-5) == "$Dart") {
    name = name.substr(0, name.length - 5);
  }
  return name;
}

/** 
 * @param {RTT}
 * @return {boolean} 
 */
RTT.nullInstanceOf = function(type) {
  return type === RTT.objectType || type === RTT.dynamicType;
};

/**
 * @param {*} value The value to retrieve type information for
 * @return {RTT} 
 */
RTT.getNativeTypeInfo = function(value) {
  if (value instanceof Array) return Array.$lookupRTT();
  switch (typeof value) {
    case 'string': return String.$lookupRTT();
    case 'number': return Number.$lookupRTT();
    case 'boolean': return Boolean.$lookupRTT();
  } 
  return RTT.placeholderType;
};

/**
 * @param {string} name
 * @param {function(RTT,Array.<RTT>)=} implementsSupplier 
 * @param {Array.<RTT>=} typeArgs
 * @return {RTT} The RTT information object
 */
RTT.create = function(name, implementsSupplier, typeArgs) {
  if (name == $cls("Object")) return RTT.objectType;
  var typekey = RTT.getTypeKey(name, typeArgs);
  var rtt = $mapLookup(RTT.types, typekey);
  if (rtt) {
    return rtt;
  }
  var classkey = RTT.getTypeKey(name);
  rtt = new RTT(classkey, typekey, typeArgs);
  RTT.types[typekey] = rtt;
  if (implementsSupplier) {
    implementsSupplier(rtt, typeArgs);
  }
  return rtt;
};

/**
 * @param {string} classkey
 * @param {Array.<(RTT|string)>=} typeargs
 * @return {string}
 */
RTT.getTypeKey = function(classkey, typeargs) {
  var key = classkey;
  if (typeargs) {
    key += "<" + typeargs.join(",") + ">";
  }
  return key;
};

/**
 * @return {*} value
 * @return {RTT} return the RTT information object for the value
 */
RTT.getTypeInfo = function(value) {
  return (value.$typeInfo) ? value.$typeInfo : RTT.getNativeTypeInfo(value);
};

/**
 * @param {Object} o
 * @param {RTT} rtt
 * Sets the RTT on the object and returns the object itself.
 */
RTT.setTypeInfo = function(o, rtt) {
  o.$typeInfo = rtt;
  return o;
};

/**
 * @param {Object} o
 * Removes any RTT from the object and returns the object itself.
 */
RTT.removeTypeInfo = function(o) {
  o.$typeInfo = null;
  return o;
};

/**
 * The typeArg array is optional
 * @param {Array.<RTT>=} typeArgs
 * @param {number} i
 * @return {RTT}
 */
RTT.getTypeArg = function(typeArgs, i) {
  if (typeArgs) {
    if (typeArgs.length > i) {
      return typeArgs[i];
    } else {
      throw new Error("Missing type arg");
    }
  } 
  return RTT.dynamicType; 
};

/**
 * The typeArg array is optional
 * @param {*} o
 * @param {string} classkey
 * @return {Array.<RTT>}
 */
RTT.getTypeArgsFor = function(o, classkey) {
  var rtt = $mapLookup(RTT.getTypeInfo(o).implementedTypes, classkey);
  if (!rtt) {
    throw new Error("internal error: can not find " +
        classkey + " in " + JSON.stringify(o));
  }
  return rtt.typeArgs;
};

// Base types for runtime type information

/** @type {!RTT} */
RTT.objectType = new RTT($cls('Object'));
RTT.objectType.implementedBy = function(o) {return true};
RTT.objectType.implementedByType = function(o) {return true};

/** @type {!RTT} */
RTT.dynamicType = new RTT($cls('Dynamic'));
RTT.dynamicType.implementedBy = function(o) {return true};
RTT.dynamicType.implementedByType = function(o) {return true};

/** @type {!RTT} */
RTT.placeholderType = new RTT($cls('::'));
RTT.placeholderType.implementedBy = function(o) {return true};
RTT.placeholderType.implementedByType = function(o) {return true};

/**
 * Checks that a value is assignable to an expected type, and either returns that
 * value if it is, or else throws a TypeMismatchException.
 *
 * @param {!RTT} the expected type
 * @param {*} the value to check
 * @return {*} the value
 */
function $chk(rtt, value) {
  // null can be assigned to any type
  if (value == $Dart$Null || rtt.implementedBy(value)) {
    return value;
  }
  $te(rtt, value);
}

/**
 * Throw a TypeError.  See core.dart for the ExceptionHelper class.
 *
 * @param {!RTT} the expected type
 * @param {*) the value that failed
 */
function $te(rtt, value) {
  var srcType = RTT.getTypeInfo(value).getClassName();
  var dstType = rtt.getClassName();
  var e = native_ExceptionHelper_createTypeError(srcType, dstType);
  $Dart$ThrowException(e);
}

// Setup the Function object
Function.prototype.$implements$Function$Dart = 1;
RTT.setTypeInfo(Function.prototype, RTT.create($cls('Function$Dart')));

/** 
 * @param {string} cls 
 * @return {string}
 * @consistentIdGenerator 
 */
function $cls(cls) {
  return "cls:" + cls;
}
// Copyright (c) 2011, the Dart project authors.  Please see the AUTHORS file
// for details. All rights reserved. Use of this source code is governed by a
// BSD-style license that can be found in the LICENSE file.

/**
 * Extend the String prototype with members expected in dart.
 */

String.$instanceOf = function(obj) {
  return typeof obj == 'string' || obj instanceof String;
};

function native_StringImplementation__indexOperator(index) {
  return this[index];
}

function native_StringImplementation__charCodeAt(index) {
  return this.charCodeAt(index);
}

function native_StringImplementation_get$length() {
  return this.length;
}

function native_StringImplementation_EQ(other) {
  if (typeof other == 'string') {
    return this == other;
  } else if (other instanceof String) {
    // Must convert other to a primitive for value equality to work.
    return this == String(other);
  } else {
    return false;
  }
}

function native_StringImplementation_indexOf(other, startIndex) {
  return this.indexOf(other, startIndex);
}

function native_StringImplementation_lastIndexOf(other, fromIndex) {
  if (other == "") {
    return Math.min(this.length, fromIndex);
  }
  return this.lastIndexOf(other, fromIndex);
}

function native_StringImplementation_concat(other) {
  return this.concat(other);
}

function native_StringImplementation__substringUnchecked(startIndex, endIndex) {
  return this.substring(startIndex, endIndex);
}

function native_StringImplementation_trim() {
  if (this.trim) return this.trim();
  return this.replace(new RegExp("^[\s]+|[\s]+$", "g"), "");
}

function native_StringImplementation__replace(from, to) {
  if (String.$instanceOf(from)) {
    return this.replace(from, to);
  } else {
    return this.replace($DartRegExpToJSRegExp(from), to);
  }
}

function native_StringImplementation__replaceAll(from, to) {
  if (String.$instanceOf(from)) {
    var regexp = new RegExp(
        from.replace(/[-[\]{}()*+?.,\\^$|#\s]/g, "\\$&"), 'g');
    return this.replace(regexp, to);
  } else {
    var regexp = $DartRegExpToJSRegExp(from);
    return this.replace(regexp, to);
  }
}

function native_StringImplementation__split(pattern) {
  if (String.$instanceOf(pattern)) {
    return this.split(pattern);
  } else {
    return this.split($DartRegExpToJSRegExp(pattern));
  }
}

function native_StringImplementation_toLowerCase() {
  return this.toLowerCase();
}

function native_StringImplementation_toUpperCase() {
  return this.toUpperCase();
}

// Inherited from Hashable.
function native_StringImplementation_hashCode() {
  if (this.hash_ === undefined) {
    for (var i = 0; i < this.length; i++) {
      var ch = this.charCodeAt(i);
      this.hash_ += ch;
      this.hash_ += this.hash_ << 10;
      this.hash_ ^= this.hash_ >> 6;
    }

    this.hash_ += this.hash_ << 3;
    this.hash_ ^= this.hash_ >> 11;
    this.hash_ += this.hash_ << 15;
    this.hash_ = this.hash_ & ((1 << 29) - 1);
  }
  return this.hash_;
}

function native_StringImplementation_toString() {
  // Return the primitive string of this String object.
  return String(this);
}

// TODO(floitsch): If we allow comparison operators on the String class we
// should move this function into dart world.
function native_StringImplementation_compareTo(other) {
  if (this == other) return 0;
  if (this < other) return -1;
  return 1;
}

function native_StringImplementation__newFromValues(array) {
  if (!(array instanceof Array)) {
    var length = native__ArrayJsUtil__arrayLength(array);
    var tmp = new Array(length);
    for (var i = 0; i < length; i++) {
      tmp[i] = INDEX$operator(array, i);
    }
    array = tmp;
  }
  return String.fromCharCode.apply(this, array);
}

// Deprecated old name of new String.fromValues(..).
function native_StringBase_createFromCharCodes(array) {
  return native_StringImplementation__newFromValues(array);
}
function ArrayFactory$Dart(){
}

ArrayFactory$Dart.$lookupRTT = function(){
  return RTT.create($cls('ArrayFactory$Dart'));
}
;
ArrayFactory$Dart.$addTo = function(target){
  var rtt = ArrayFactory$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
ArrayFactory$Dart.prototype.$implements$ArrayFactory$Dart = 1;
ArrayFactory$Dart.prototype.$implements$Object$Dart = 1;
ArrayFactory$Dart.Array$from$5$Factory = function($typeArgs, other){
  var array = ArrayFactory$Dart.Array$$Factory([RTT.getTypeArg($typeArgs, 0)], $Dart$Null);
  {
    var $0 = other.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var e = $0.next$named(0, $noargs);
      {
        array.add$named(1, $noargs, e);
      }
    }
  }
  return array;
}
;
ArrayFactory$Dart.Array$fromArray$5$Factory = function($typeArgs, other, startIndex, endIndex){
  var tmp$0;
  var array = ArrayFactory$Dart.Array$$Factory([RTT.getTypeArg($typeArgs, 0)], $Dart$Null);
  if (GT$operator(endIndex, other.length$getter())) {
    endIndex = other.length$getter();
  }
  if (LT$operator(startIndex, 0)) {
    startIndex = 0;
  }
  var count = SUB$operator(endIndex, startIndex);
  if (GT$operator(count, 0)) {
    array.length$setter(tmp$0 = count) , tmp$0;
    Arrays$Dart.copy$member(other, startIndex, array, 0, count);
  }
  return array;
}
;
ArrayFactory$Dart.Array$$Factory = function($typeArgs, length_0){
  var tmp$0;
  var isFixed = true;
  if (length_0 == null) {
    length_0 = 0;
    isFixed = false;
  }
   else {
    if (LT$operator(length_0, 0)) {
      $Dart$ThrowException(IllegalArgumentException$Dart.IllegalArgumentException$$Factory(length_0));
    }
  }
  var array = ArrayFactory$Dart._new$$member_(TypeToken$Dart.TypeToken$$Factory(TypeToken$Dart.$lookupRTT([RTT.getTypeArg($typeArgs, 0)])), length_0);
  array._isFixed$$setter_(tmp$0 = isFixed) , tmp$0;
  return array;
}
;
ArrayFactory$Dart._new$$member_ = function(typeToken, length_0){
  return native_ArrayFactory__new(typeToken, length_0);
}
;
ArrayFactory$Dart._new$$named_ = function($n, $o, typeToken, length_0){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return ArrayFactory$Dart._new$$member_(typeToken, length_0);
}
;
ArrayFactory$Dart._new$$getter_ = function _new$$getter_(){
  return ArrayFactory$Dart._new$$named_;
}
;
function ListFactory$Dart(){
}

ListFactory$Dart.$lookupRTT = function(){
  return RTT.create($cls('ListFactory$Dart'));
}
;
ListFactory$Dart.$addTo = function(target){
  var rtt = ListFactory$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
ListFactory$Dart.prototype.$implements$ListFactory$Dart = 1;
ListFactory$Dart.prototype.$implements$Object$Dart = 1;
ListFactory$Dart.List$from$4$Factory = function($typeArgs, other){
  var list = ListFactory$Dart.List$$Factory([RTT.getTypeArg($typeArgs, 0)], $Dart$Null);
  {
    var $0 = other.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var e = $0.next$named(0, $noargs);
      {
        list.add$named(1, $noargs, e);
      }
    }
  }
  return list;
}
;
ListFactory$Dart.List$fromList$4$Factory = function($typeArgs, other, startIndex, endIndex){
  var tmp$0;
  var list = ListFactory$Dart.List$$Factory([RTT.getTypeArg($typeArgs, 0)], $Dart$Null);
  if (GT$operator(endIndex, other.length$getter())) {
    endIndex = other.length$getter();
  }
  if (LT$operator(startIndex, 0)) {
    startIndex = 0;
  }
  var count = SUB$operator(endIndex, startIndex);
  if (GT$operator(count, 0)) {
    list.length$setter(tmp$0 = count) , tmp$0;
    Arrays$Dart.copy$member(other, startIndex, list, 0, count);
  }
  return list;
}
;
ListFactory$Dart.List$$Factory = function($typeArgs, length_0){
  var tmp$0;
  var isFixed = true;
  if (length_0 == null) {
    length_0 = 0;
    isFixed = false;
  }
   else {
    if (LT$operator(length_0, 0)) {
      $Dart$ThrowException(IllegalArgumentException$Dart.IllegalArgumentException$$Factory(length_0));
    }
  }
  var list = ListFactory$Dart._new$$member_(TypeToken$Dart.TypeToken$$Factory(TypeToken$Dart.$lookupRTT([RTT.getTypeArg($typeArgs, 0)])), length_0);
  list._isFixed$$setter_(tmp$0 = isFixed) , tmp$0;
  return list;
}
;
ListFactory$Dart._new$$member_ = function(typeToken, length_0){
  return native_ListFactory__new(typeToken, length_0);
}
;
ListFactory$Dart._new$$named_ = function($n, $o, typeToken, length_0){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return ListFactory$Dart._new$$member_(typeToken, length_0);
}
;
ListFactory$Dart._new$$getter_ = function _new$$getter_(){
  return ListFactory$Dart._new$$named_;
}
;
Array.$lookupRTT = function(typeArgs){
  return RTT.create($cls('Array'), Array.$RTTimplements, typeArgs);
}
;
Array.$RTTimplements = function(rtt, typeArgs){
  Array.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
Array.$addTo = function(target, typeArgs){
  var rtt = Array.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  Array$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0)]);
}
;
Array.prototype.$implements$ObjectArray$Dart = 1;
Array.prototype.$implements$Array$Dart = 1;
Array.prototype.$implements$List$Dart = 1;
Array.prototype.$implements$Collection$Dart = 1;
Array.prototype.$implements$Iterable$Dart = 1;
Array.prototype.$implements$Object$Dart = 1;
Array.prototype._isFixed$$named_ = function(){
  return this._isFixed$$getter_().apply(this, arguments);
}
;
Array.prototype._isFixed$$getter_ = function(){
  return this._isFixed$$field_;
}
;
Array.prototype._isFixed$$setter_ = function(tmp$0){
  this._isFixed$$field_ = tmp$0;
}
;
Array.prototype.INDEX$operator = function(index){
  if (LTE$operator(0, index) && LT$operator(index, this.length$getter())) {
    return this._indexOperator$$member_(index);
  }
  $Dart$ThrowException(IndexOutOfRangeException$Dart.IndexOutOfRangeException$$Factory(index));
}
;
Array.prototype.ASSIGN_INDEX$operator = function(index, value){
  if (LT$operator(index, 0) || LTE$operator(this.length$getter(), index)) {
    $Dart$ThrowException(IndexOutOfRangeException$Dart.IndexOutOfRangeException$$Factory(index));
  }
  this._indexAssignOperator$$member_(index, value);
}
;
Array.prototype.iterator$member = function(){
  if (this._isFixed$$getter_()) {
    return FixedSizeArrayIterator$Dart.FixedSizeArrayIterator$$Factory(FixedSizeArrayIterator$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('Array')), 0)]), this);
  }
   else {
    return VariableSizeArrayIterator$Dart.VariableSizeArrayIterator$$Factory(VariableSizeArrayIterator$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('Array')), 0)]), this);
  }
}
;
Array.prototype.iterator$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Array.prototype.iterator$member.call(this);
}
;
Array.prototype.iterator$getter = function iterator$getter(){
  return $bind(Array.prototype.iterator$named, this);
}
;
Array.prototype._indexOperator$$member_ = function(index){
  return native_ObjectArray__indexOperator.call(this, index);
}
;
Array.prototype._indexOperator$$named_ = function($n, $o, index){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Array.prototype._indexOperator$$member_.call(this, index);
}
;
Array.prototype._indexOperator$$getter_ = function _indexOperator$$getter_(){
  return $bind(Array.prototype._indexOperator$$named_, this);
}
;
Array.prototype._indexAssignOperator$$member_ = function(index, value){
  return native_ObjectArray__indexAssignOperator.call(this, index, value);
}
;
Array.prototype._indexAssignOperator$$named_ = function($n, $o, index, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Array.prototype._indexAssignOperator$$member_.call(this, index, value);
}
;
Array.prototype._indexAssignOperator$$getter_ = function _indexAssignOperator$$getter_(){
  return $bind(Array.prototype._indexAssignOperator$$named_, this);
}
;
Array.prototype.length$named = function(){
  return this.length$getter().apply(this, arguments);
}
;
Array.prototype.length$getter = function(){
  return native_ObjectArray_get$length.call(this);
}
;
Array.prototype.length$setter = function(length_0){
  if (this._isFixed$$getter_()) {
    $Dart$ThrowException($intern(UnsupportedOperationException$Dart.UnsupportedOperationException$$Factory('Cannot change the length of a non-extendable array')));
  }
   else {
    this._setLength$$member_(length_0);
  }
}
;
Array.prototype._setLength$$member_ = function(length_0){
  return native_ObjectArray__setLength.call(this, length_0);
}
;
Array.prototype._setLength$$named_ = function($n, $o, length_0){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Array.prototype._setLength$$member_.call(this, length_0);
}
;
Array.prototype._setLength$$getter_ = function _setLength$$getter_(){
  return $bind(Array.prototype._setLength$$named_, this);
}
;
Array.prototype._add$$member_ = function(value){
  return native_ObjectArray__add.call(this, value);
}
;
Array.prototype._add$$named_ = function($n, $o, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Array.prototype._add$$member_.call(this, value);
}
;
Array.prototype._add$$getter_ = function _add$$getter_(){
  return $bind(Array.prototype._add$$named_, this);
}
;
Array.prototype.forEach$member = function(f){
  Collections$Dart.forEach$member(this, f);
}
;
Array.prototype.forEach$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Array.prototype.forEach$member.call(this, f);
}
;
Array.prototype.forEach$getter = function forEach$getter(){
  return $bind(Array.prototype.forEach$named, this);
}
;
Array.prototype.filter$member = function(f){
  return Collections$Dart.filter$member(this, ArrayFactory$Dart.Array$$Factory([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('Array')), 0)], $Dart$Null), f);
}
;
Array.prototype.filter$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Array.prototype.filter$member.call(this, f);
}
;
Array.prototype.filter$getter = function filter$getter(){
  return $bind(Array.prototype.filter$named, this);
}
;
Array.prototype.every$member = function(f){
  return Collections$Dart.every$member(this, f);
}
;
Array.prototype.every$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Array.prototype.every$member.call(this, f);
}
;
Array.prototype.every$getter = function every$getter(){
  return $bind(Array.prototype.every$named, this);
}
;
Array.prototype.some$member = function(f){
  return Collections$Dart.some$member(this, f);
}
;
Array.prototype.some$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Array.prototype.some$member.call(this, f);
}
;
Array.prototype.some$getter = function some$getter(){
  return $bind(Array.prototype.some$named, this);
}
;
Array.prototype.isEmpty$member = function(){
  return EQ$operator(this.length$getter(), 0);
}
;
Array.prototype.isEmpty$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Array.prototype.isEmpty$member.call(this);
}
;
Array.prototype.isEmpty$getter = function isEmpty$getter(){
  return $bind(Array.prototype.isEmpty$named, this);
}
;
Array.prototype.sort$member = function(compare){
  DualPivotQuicksort$Dart.sort$member(this, compare);
}
;
Array.prototype.sort$named = function($n, $o, compare){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Array.prototype.sort$member.call(this, compare);
}
;
Array.prototype.sort$getter = function sort$getter(){
  return $bind(Array.prototype.sort$named, this);
}
;
Array.prototype.copyFrom$member = function(src, srcStart, dstStart, count){
  Arrays$Dart.copy$member(src, srcStart, this, dstStart, count);
}
;
Array.prototype.copyFrom$named = function($n, $o, src, srcStart, dstStart, count){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 4)
    $nsme();
  return Array.prototype.copyFrom$member.call(this, src, srcStart, dstStart, count);
}
;
Array.prototype.copyFrom$getter = function copyFrom$getter(){
  return $bind(Array.prototype.copyFrom$named, this);
}
;
Array.prototype.setRange$member = function(start, length_0, from, startFrom){
  $Dart$ThrowException($intern(NotImplementedException$Dart.NotImplementedException$$Factory()));
}
;
Array.prototype.setRange$named = function($n, $o, start, length_0, from, startFrom){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 3:
      startFrom = $o.startFrom?(++seen , $o.startFrom):(++def , 0);
  }
  if (seen != $o.count || seen + def + $n != 4)
    $nsme();
  return Array.prototype.setRange$member.call(this, start, length_0, from, startFrom);
}
;
Array.prototype.setRange$getter = function setRange$getter(){
  return $bind(Array.prototype.setRange$named, this);
}
;
Array.prototype.removeRange$member = function(start, length_0){
  $Dart$ThrowException($intern(NotImplementedException$Dart.NotImplementedException$$Factory()));
}
;
Array.prototype.removeRange$named = function($n, $o, start, length_0){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Array.prototype.removeRange$member.call(this, start, length_0);
}
;
Array.prototype.removeRange$getter = function removeRange$getter(){
  return $bind(Array.prototype.removeRange$named, this);
}
;
Array.prototype.insertRange$member = function(start, length_0, initialValue){
  $Dart$ThrowException($intern(NotImplementedException$Dart.NotImplementedException$$Factory()));
}
;
Array.prototype.insertRange$named = function($n, $o, start, length_0, initialValue){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 2:
      initialValue = $o.initialValue?(++seen , $o.initialValue):(++def , $Dart$Null);
  }
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return Array.prototype.insertRange$member.call(this, start, length_0, initialValue);
}
;
Array.prototype.insertRange$getter = function insertRange$getter(){
  return $bind(Array.prototype.insertRange$named, this);
}
;
Array.prototype.getRange$member = function(start, length_0){
  $Dart$ThrowException($intern(NotImplementedException$Dart.NotImplementedException$$Factory()));
}
;
Array.prototype.getRange$named = function($n, $o, start, length_0){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Array.prototype.getRange$member.call(this, start, length_0);
}
;
Array.prototype.getRange$getter = function getRange$getter(){
  return $bind(Array.prototype.getRange$named, this);
}
;
Array.prototype.indexOf$member = function(element, startIndex){
  return Arrays$Dart.indexOf$member(this, element, startIndex, this.length$getter());
}
;
Array.prototype.indexOf$named = function($n, $o, element, startIndex){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Array.prototype.indexOf$member.call(this, element, startIndex);
}
;
Array.prototype.indexOf$getter = function indexOf$getter(){
  return $bind(Array.prototype.indexOf$named, this);
}
;
Array.prototype.lastIndexOf$member = function(element, startIndex){
  return Arrays$Dart.lastIndexOf$member(this, element, startIndex);
}
;
Array.prototype.lastIndexOf$named = function($n, $o, element, startIndex){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Array.prototype.lastIndexOf$member.call(this, element, startIndex);
}
;
Array.prototype.lastIndexOf$getter = function lastIndexOf$getter(){
  return $bind(Array.prototype.lastIndexOf$named, this);
}
;
Array.prototype.add$member = function(element){
  if (this._isFixed$$getter_()) {
    $Dart$ThrowException($intern(UnsupportedOperationException$Dart.UnsupportedOperationException$$Factory('Cannot add to a non-extendable array')));
  }
   else {
    this._add$$member_(element);
  }
}
;
Array.prototype.add$named = function($n, $o, element){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Array.prototype.add$member.call(this, element);
}
;
Array.prototype.add$getter = function add$getter(){
  return $bind(Array.prototype.add$named, this);
}
;
Array.prototype.addLast$member = function(element){
  this.add$member(element);
}
;
Array.prototype.addLast$named = function($n, $o, element){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Array.prototype.addLast$member.call(this, element);
}
;
Array.prototype.addLast$getter = function addLast$getter(){
  return $bind(Array.prototype.addLast$named, this);
}
;
Array.prototype.addAll$member = function(elements){
  if (this._isFixed$$getter_()) {
    $Dart$ThrowException($intern(UnsupportedOperationException$Dart.UnsupportedOperationException$$Factory('Cannot add to a non-extendable array')));
  }
   else {
    {
      var $0 = elements.iterator$named(0, $noargs);
      while ($0.hasNext$named(0, $noargs)) {
        var e = $0.next$named(0, $noargs);
        {
          this._add$$member_(e);
        }
      }
    }
  }
}
;
Array.prototype.addAll$named = function($n, $o, elements){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Array.prototype.addAll$member.call(this, elements);
}
;
Array.prototype.addAll$getter = function addAll$getter(){
  return $bind(Array.prototype.addAll$named, this);
}
;
Array.prototype.clear$member = function(){
  var tmp$0;
  if (this._isFixed$$getter_()) {
    $Dart$ThrowException($intern(UnsupportedOperationException$Dart.UnsupportedOperationException$$Factory('Cannot clear a non-extendable array')));
  }
   else {
    this.length$setter(tmp$0 = 0) , tmp$0;
  }
}
;
Array.prototype.clear$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Array.prototype.clear$member.call(this);
}
;
Array.prototype.clear$getter = function clear$getter(){
  return $bind(Array.prototype.clear$named, this);
}
;
Array.prototype.removeLast$member = function(){
  var tmp$0;
  if (this._isFixed$$getter_()) {
    $Dart$ThrowException($intern(UnsupportedOperationException$Dart.UnsupportedOperationException$$Factory('Cannot remove in a non-extendable array')));
  }
   else {
    var element = this.last$member();
    this.length$setter(tmp$0 = SUB$operator(this.length$getter(), 1)) , tmp$0;
    return element;
  }
}
;
Array.prototype.removeLast$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Array.prototype.removeLast$member.call(this);
}
;
Array.prototype.removeLast$getter = function removeLast$getter(){
  return $bind(Array.prototype.removeLast$named, this);
}
;
Array.prototype.last$member = function(){
  return this.INDEX$operator(SUB$operator(this.length$getter(), 1));
}
;
Array.prototype.last$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Array.prototype.last$member.call(this);
}
;
Array.prototype.last$getter = function last$getter(){
  return $bind(Array.prototype.last$named, this);
}
;
Array.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
Array.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
Array.ObjectArray$$Factory = function($rtt){
  var tmp$0 = new Array;
  tmp$0.$typeInfo = $rtt;
  Array.$Initializer.call(tmp$0);
  Array.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function FixedSizeArrayIterator$Dart(){
}

FixedSizeArrayIterator$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('FixedSizeArrayIterator$Dart'), FixedSizeArrayIterator$Dart.$RTTimplements, typeArgs);
}
;
FixedSizeArrayIterator$Dart.$RTTimplements = function(rtt, typeArgs){
  FixedSizeArrayIterator$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
FixedSizeArrayIterator$Dart.$addTo = function(target, typeArgs){
  var rtt = FixedSizeArrayIterator$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  VariableSizeArrayIterator$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0)]);
}
;
FixedSizeArrayIterator$Dart.prototype.$implements$FixedSizeArrayIterator$Dart = 1;
FixedSizeArrayIterator$Dart.prototype.$implements$VariableSizeArrayIterator$Dart = 1;
FixedSizeArrayIterator$Dart.prototype.$implements$Iterator$Dart = 1;
FixedSizeArrayIterator$Dart.prototype.$implements$Object$Dart = 1;
$inherits(FixedSizeArrayIterator$Dart, VariableSizeArrayIterator$Dart);
FixedSizeArrayIterator$Dart.$Constructor = function(array){
  VariableSizeArrayIterator$Dart.$Constructor.call(this, array);
}
;
FixedSizeArrayIterator$Dart.$Initializer = function(array){
  VariableSizeArrayIterator$Dart.$Initializer.call(this, array);
  this._length$$field_ = array.length$getter();
}
;
FixedSizeArrayIterator$Dart.FixedSizeArrayIterator$$Factory = function($rtt, array){
  var tmp$0 = new FixedSizeArrayIterator$Dart;
  tmp$0.$typeInfo = $rtt;
  FixedSizeArrayIterator$Dart.$Initializer.call(tmp$0, array);
  FixedSizeArrayIterator$Dart.$Constructor.call(tmp$0, array);
  return tmp$0;
}
;
FixedSizeArrayIterator$Dart.prototype.hasNext$member = function(){
  return GT$operator(this._length$$getter_(), this._pos$$getter_());
}
;
FixedSizeArrayIterator$Dart.prototype.hasNext$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return FixedSizeArrayIterator$Dart.prototype.hasNext$member.call(this);
}
;
FixedSizeArrayIterator$Dart.prototype.hasNext$getter = function hasNext$getter(){
  return $bind(FixedSizeArrayIterator$Dart.prototype.hasNext$named, this);
}
;
FixedSizeArrayIterator$Dart.prototype._length$$named_ = function(){
  return this._length$$getter_().apply(this, arguments);
}
;
FixedSizeArrayIterator$Dart.prototype._length$$getter_ = function(){
  return this._length$$field_;
}
;
function VariableSizeArrayIterator$Dart(){
}

VariableSizeArrayIterator$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('VariableSizeArrayIterator$Dart'), VariableSizeArrayIterator$Dart.$RTTimplements, typeArgs);
}
;
VariableSizeArrayIterator$Dart.$RTTimplements = function(rtt, typeArgs){
  VariableSizeArrayIterator$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
VariableSizeArrayIterator$Dart.$addTo = function(target, typeArgs){
  var rtt = VariableSizeArrayIterator$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  Iterator$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0)]);
}
;
VariableSizeArrayIterator$Dart.prototype.$implements$VariableSizeArrayIterator$Dart = 1;
VariableSizeArrayIterator$Dart.prototype.$implements$Iterator$Dart = 1;
VariableSizeArrayIterator$Dart.prototype.$implements$Object$Dart = 1;
VariableSizeArrayIterator$Dart.$Constructor = function(array){
  Object.$Constructor.call(this);
}
;
VariableSizeArrayIterator$Dart.$Initializer = function(array){
  Object.$Initializer.call(this);
  this._array$$field_ = array;
  this._pos$$field_ = 0;
}
;
VariableSizeArrayIterator$Dart.VariableSizeArrayIterator$$Factory = function($rtt, array){
  var tmp$0 = new VariableSizeArrayIterator$Dart;
  tmp$0.$typeInfo = $rtt;
  VariableSizeArrayIterator$Dart.$Initializer.call(tmp$0, array);
  VariableSizeArrayIterator$Dart.$Constructor.call(tmp$0, array);
  return tmp$0;
}
;
VariableSizeArrayIterator$Dart.prototype.hasNext$member = function(){
  return GT$operator(this._array$$getter_().length$getter(), this._pos$$getter_());
}
;
VariableSizeArrayIterator$Dart.prototype.hasNext$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return VariableSizeArrayIterator$Dart.prototype.hasNext$member.call(this);
}
;
VariableSizeArrayIterator$Dart.prototype.hasNext$getter = function hasNext$getter(){
  return $bind(VariableSizeArrayIterator$Dart.prototype.hasNext$named, this);
}
;
VariableSizeArrayIterator$Dart.prototype.next$member = function(){
  var tmp$1, tmp$0;
  if (!this.hasNext$member()) {
    $Dart$ThrowException($intern(NoMoreElementsException$Dart.NoMoreElementsException$$Factory()));
  }
  return this._array$$getter_().INDEX$operator((tmp$0 = this._pos$$getter_() , (this._pos$$setter_(tmp$1 = ADD$operator(tmp$0, 1)) , tmp$1 , tmp$0)));
}
;
VariableSizeArrayIterator$Dart.prototype.next$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return VariableSizeArrayIterator$Dart.prototype.next$member.call(this);
}
;
VariableSizeArrayIterator$Dart.prototype.next$getter = function next$getter(){
  return $bind(VariableSizeArrayIterator$Dart.prototype.next$named, this);
}
;
VariableSizeArrayIterator$Dart.prototype._array$$named_ = function(){
  return this._array$$getter_().apply(this, arguments);
}
;
VariableSizeArrayIterator$Dart.prototype._array$$getter_ = function(){
  return this._array$$field_;
}
;
VariableSizeArrayIterator$Dart.prototype._pos$$named_ = function(){
  return this._pos$$getter_().apply(this, arguments);
}
;
VariableSizeArrayIterator$Dart.prototype._pos$$getter_ = function(){
  return this._pos$$field_;
}
;
VariableSizeArrayIterator$Dart.prototype._pos$$setter_ = function(tmp$0){
  this._pos$$field_ = tmp$0;
}
;
function _ArrayJsUtil$Dart(){
}

_ArrayJsUtil$Dart.$lookupRTT = function(){
  return RTT.create($cls('_ArrayJsUtil$Dart'));
}
;
_ArrayJsUtil$Dart.$addTo = function(target){
  var rtt = _ArrayJsUtil$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
_ArrayJsUtil$Dart.prototype.$implements$_ArrayJsUtil$Dart = 1;
_ArrayJsUtil$Dart.prototype.$implements$Object$Dart = 1;
_ArrayJsUtil$Dart._arrayLength$$member_ = function(array){
  return array.length$getter();
}
;
_ArrayJsUtil$Dart._arrayLength$$named_ = function($n, $o, array){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _ArrayJsUtil$Dart._arrayLength$$member_(array);
}
;
function native__ArrayJsUtil__arrayLength(array){
  return _ArrayJsUtil$Dart._arrayLength$$member_(array);
}

_ArrayJsUtil$Dart._arrayLength$$getter_ = function _arrayLength$$getter_(){
  return _ArrayJsUtil$Dart._arrayLength$$named_;
}
;
_ArrayJsUtil$Dart._newArray$$member_ = function(len){
  return ArrayFactory$Dart.Array$$Factory(null, len);
}
;
_ArrayJsUtil$Dart._newArray$$named_ = function($n, $o, len){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _ArrayJsUtil$Dart._newArray$$member_(len);
}
;
function native__ArrayJsUtil__newArray(len){
  return _ArrayJsUtil$Dart._newArray$$member_(len);
}

_ArrayJsUtil$Dart._newArray$$getter_ = function _newArray$$getter_(){
  return _ArrayJsUtil$Dart._newArray$$named_;
}
;
_ArrayJsUtil$Dart._throwIndexOutOfRangeException$$member_ = function(index){
  $Dart$ThrowException(IndexOutOfRangeException$Dart.IndexOutOfRangeException$$Factory(index));
}
;
_ArrayJsUtil$Dart._throwIndexOutOfRangeException$$named_ = function($n, $o, index){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _ArrayJsUtil$Dart._throwIndexOutOfRangeException$$member_(index);
}
;
function native__ArrayJsUtil__throwIndexOutOfRangeException(index){
  return _ArrayJsUtil$Dart._throwIndexOutOfRangeException$$member_(index);
}

_ArrayJsUtil$Dart._throwIndexOutOfRangeException$$getter_ = function _throwIndexOutOfRangeException$$getter_(){
  return _ArrayJsUtil$Dart._throwIndexOutOfRangeException$$named_;
}
;
_ArrayJsUtil$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
_ArrayJsUtil$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
_ArrayJsUtil$Dart._ArrayJsUtil$$Factory = function(){
  var tmp$0 = new _ArrayJsUtil$Dart;
  tmp$0.$typeInfo = _ArrayJsUtil$Dart.$lookupRTT();
  _ArrayJsUtil$Dart.$Initializer.call(tmp$0);
  _ArrayJsUtil$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function Arrays$Dart(){
}

Arrays$Dart.$lookupRTT = function(){
  return RTT.create($cls('Arrays$Dart'));
}
;
Arrays$Dart.$addTo = function(target){
  var rtt = Arrays$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
Arrays$Dart.prototype.$implements$Arrays$Dart = 1;
Arrays$Dart.prototype.$implements$Object$Dart = 1;
Arrays$Dart.copy$member = function(src, srcStart, dst, dstStart, count){
  var tmp$5, tmp$1, tmp$2, tmp$3, tmp$4, tmp$0;
  if (srcStart == null) {
    srcStart = 0;
  }
  if (dstStart == null) {
    dstStart = 0;
  }
  if (LT$operator(srcStart, dstStart)) {
    {
      var i = SUB$operator(ADD$operator(srcStart, count), 1);
      var j = SUB$operator(ADD$operator(dstStart, count), 1);
      for (; GTE$operator(i, srcStart); tmp$1 = i , (i = SUB$operator(tmp$1, 1) , tmp$1) , (tmp$0 = j , (j = SUB$operator(tmp$0, 1) , tmp$0))) {
        dst.ASSIGN_INDEX$operator(j, tmp$2 = src.INDEX$operator(i)) , tmp$2;
      }
    }
  }
   else {
    {
      var i_0 = srcStart;
      var j_0 = dstStart;
      for (; LT$operator(i_0, ADD$operator(srcStart, count)); tmp$4 = i_0 , (i_0 = ADD$operator(tmp$4, 1) , tmp$4) , (tmp$3 = j_0 , (j_0 = ADD$operator(tmp$3, 1) , tmp$3))) {
        dst.ASSIGN_INDEX$operator(j_0, tmp$5 = src.INDEX$operator(i_0)) , tmp$5;
      }
    }
  }
}
;
Arrays$Dart.copy$named = function($n, $o, src, srcStart, dst, dstStart, count){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 5)
    $nsme();
  return Arrays$Dart.copy$member(src, srcStart, dst, dstStart, count);
}
;
Arrays$Dart.copy$getter = function copy$getter(){
  return Arrays$Dart.copy$named;
}
;
Arrays$Dart.indexOf$member = function(a, element, startIndex, endIndex){
  var tmp$0;
  if (GTE$operator(startIndex, a.length$getter())) {
    return negate$operator(1);
  }
  if (LT$operator(startIndex, 0)) {
    startIndex = 0;
  }
  {
    var i = startIndex;
    for (; LT$operator(i, endIndex); tmp$0 = i , (i = ADD$operator(tmp$0, 1) , tmp$0)) {
      if (EQ$operator(a.INDEX$operator(i), element)) {
        return i;
      }
    }
  }
  return negate$operator(1);
}
;
Arrays$Dart.indexOf$named = function($n, $o, a, element, startIndex, endIndex){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 4)
    $nsme();
  return Arrays$Dart.indexOf$member(a, element, startIndex, endIndex);
}
;
Arrays$Dart.indexOf$getter = function indexOf$getter(){
  return Arrays$Dart.indexOf$named;
}
;
Arrays$Dart.lastIndexOf$member = function(a, element, startIndex){
  var tmp$0;
  if (LT$operator(startIndex, 0)) {
    return negate$operator(1);
  }
  if (GTE$operator(startIndex, a.length$getter())) {
    startIndex = SUB$operator(a.length$getter(), 1);
  }
  {
    var i = startIndex;
    for (; GTE$operator(i, 0); tmp$0 = i , (i = SUB$operator(tmp$0, 1) , tmp$0)) {
      if (EQ$operator(a.INDEX$operator(i), element)) {
        return i;
      }
    }
  }
  return negate$operator(1);
}
;
Arrays$Dart.lastIndexOf$named = function($n, $o, a, element, startIndex){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return Arrays$Dart.lastIndexOf$member(a, element, startIndex);
}
;
Arrays$Dart.lastIndexOf$getter = function lastIndexOf$getter(){
  return Arrays$Dart.lastIndexOf$named;
}
;
Arrays$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
Arrays$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
Arrays$Dart.Arrays$$Factory = function(){
  var tmp$0 = new Arrays$Dart;
  tmp$0.$typeInfo = Arrays$Dart.$lookupRTT();
  Arrays$Dart.$Initializer.call(tmp$0);
  Arrays$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
Boolean.$lookupRTT = function(){
  return RTT.create($cls('Boolean'), Boolean.$RTTimplements);
}
;
Boolean.$RTTimplements = function(rtt){
  Boolean.$addTo(rtt);
}
;
Boolean.$addTo = function(target){
  var rtt = Boolean.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  bool$Dart.$addTo(target);
}
;
Boolean.prototype.$implements$BoolImplementation$Dart = 1;
Boolean.prototype.$implements$bool$Dart = 1;
Boolean.prototype.$implements$Object$Dart = 1;
Boolean.prototype.EQ$operator = function(other){
  return native_BoolImplementation_EQ.call(this, other);
}
;
Boolean.prototype.toString$member = function(){
  return native_BoolImplementation_toString.call(this);
}
;
Boolean.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Boolean.prototype.toString$member.call(this);
}
;
Boolean.prototype.toString$getter = function toString$getter(){
  return $bind(Boolean.prototype.toString$named, this);
}
;
Boolean.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
Boolean.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
Boolean.BoolImplementation$$Factory = function(){
  var tmp$0 = new Boolean;
  tmp$0.$typeInfo = Boolean.$lookupRTT();
  Boolean.$Initializer.call(tmp$0);
  Boolean.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function Collections$Dart(){
}

Collections$Dart.$lookupRTT = function(){
  return RTT.create($cls('Collections$Dart'));
}
;
Collections$Dart.$addTo = function(target){
  var rtt = Collections$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
Collections$Dart.prototype.$implements$Collections$Dart = 1;
Collections$Dart.prototype.$implements$Object$Dart = 1;
Collections$Dart.forEach$member = function(iterable, f){
  {
    var $0 = iterable.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var e = $0.next$named(0, $noargs);
      {
        f(1, $noargs, e);
      }
    }
  }
}
;
Collections$Dart.forEach$named = function($n, $o, iterable, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Collections$Dart.forEach$member(iterable, f);
}
;
Collections$Dart.forEach$getter = function forEach$getter(){
  return Collections$Dart.forEach$named;
}
;
Collections$Dart.some$member = function(iterable, f){
  {
    var $0 = iterable.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var e = $0.next$named(0, $noargs);
      {
        if (f(1, $noargs, e)) {
          return true;
        }
      }
    }
  }
  return false;
}
;
Collections$Dart.some$named = function($n, $o, iterable, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Collections$Dart.some$member(iterable, f);
}
;
Collections$Dart.some$getter = function some$getter(){
  return Collections$Dart.some$named;
}
;
Collections$Dart.every$member = function(iterable, f){
  {
    var $0 = iterable.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var e = $0.next$named(0, $noargs);
      {
        if (!f(1, $noargs, e)) {
          return false;
        }
      }
    }
  }
  return true;
}
;
Collections$Dart.every$named = function($n, $o, iterable, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Collections$Dart.every$member(iterable, f);
}
;
Collections$Dart.every$getter = function every$getter(){
  return Collections$Dart.every$named;
}
;
Collections$Dart.filter$member = function(source, destination, f){
  {
    var $0 = source.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var e = $0.next$named(0, $noargs);
      {
        if (f(1, $noargs, e)) {
          destination.add$named(1, $noargs, e);
        }
      }
    }
  }
  return destination;
}
;
Collections$Dart.filter$named = function($n, $o, source, destination, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return Collections$Dart.filter$member(source, destination, f);
}
;
Collections$Dart.filter$getter = function filter$getter(){
  return Collections$Dart.filter$named;
}
;
Collections$Dart.isEmpty$member = function(iterable){
  return !iterable.iterator$named(0, $noargs).hasNext$named(0, $noargs);
}
;
Collections$Dart.isEmpty$named = function($n, $o, iterable){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Collections$Dart.isEmpty$member(iterable);
}
;
Collections$Dart.isEmpty$getter = function isEmpty$getter(){
  return Collections$Dart.isEmpty$named;
}
;
Collections$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
Collections$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
Collections$Dart.Collections$$Factory = function(){
  var tmp$0 = new Collections$Dart;
  tmp$0.$typeInfo = Collections$Dart.$lookupRTT();
  Collections$Dart.$Initializer.call(tmp$0);
  Collections$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function ConstHelper$Dart(){
}

ConstHelper$Dart.$lookupRTT = function(){
  return RTT.create($cls('ConstHelper$Dart'));
}
;
ConstHelper$Dart.$addTo = function(target){
  var rtt = ConstHelper$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
ConstHelper$Dart.prototype.$implements$ConstHelper$Dart = 1;
ConstHelper$Dart.prototype.$implements$Object$Dart = 1;
ConstHelper$Dart.getConstId$member = function(o){
  return native_ConstHelper_getConstId(o);
}
;
ConstHelper$Dart.getConstId$named = function($n, $o, o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return ConstHelper$Dart.getConstId$member(o);
}
;
ConstHelper$Dart.getConstId$getter = function getConstId$getter(){
  return ConstHelper$Dart.getConstId$named;
}
;
ConstHelper$Dart.getConstMapId$member = function(map){
  var sb = StringBufferImpl$Dart.StringBufferImpl$$Factory('');
  sb.add$named(1, $noargs, 'm');
  var first = true;
  {
    var $0 = map.getKeys$named(0, $noargs).iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var key = $0.next$named(0, $noargs);
      {
        if (first) {
          first = false;
        }
         else {
          sb.add$named(1, $noargs, ',');
        }
        sb.add$named(1, $noargs, ConstHelper$Dart.getConstId$member(key));
        sb.add$named(1, $noargs, ',');
        sb.add$named(1, $noargs, ConstHelper$Dart.getConstId$member(map.INDEX$operator(key)));
      }
    }
  }
  return sb.toString$named(0, $noargs);
}
;
ConstHelper$Dart.getConstMapId$named = function($n, $o, map){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return ConstHelper$Dart.getConstMapId$member(map);
}
;
function native_ConstHelper_getConstMapId(map){
  return ConstHelper$Dart.getConstMapId$member(map);
}

ConstHelper$Dart.getConstMapId$getter = function getConstMapId$getter(){
  return ConstHelper$Dart.getConstMapId$named;
}
;
ConstHelper$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
ConstHelper$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
ConstHelper$Dart.ConstHelper$$Factory = function(){
  var tmp$0 = new ConstHelper$Dart;
  tmp$0.$typeInfo = ConstHelper$Dart.$lookupRTT();
  ConstHelper$Dart.$Initializer.call(tmp$0);
  ConstHelper$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function ExceptionHelper$Dart(){
}

ExceptionHelper$Dart.$lookupRTT = function(){
  return RTT.create($cls('ExceptionHelper$Dart'));
}
;
ExceptionHelper$Dart.$addTo = function(target){
  var rtt = ExceptionHelper$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
ExceptionHelper$Dart.prototype.$implements$ExceptionHelper$Dart = 1;
ExceptionHelper$Dart.prototype.$implements$Object$Dart = 1;
ExceptionHelper$Dart.createNullPointerException$member = function(){
  return NullPointerException$Dart.NullPointerException$$Factory();
}
;
ExceptionHelper$Dart.createNullPointerException$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return ExceptionHelper$Dart.createNullPointerException$member();
}
;
function native_ExceptionHelper_createNullPointerException(){
  return ExceptionHelper$Dart.createNullPointerException$member();
}

ExceptionHelper$Dart.createNullPointerException$getter = function createNullPointerException$getter(){
  return ExceptionHelper$Dart.createNullPointerException$named;
}
;
ExceptionHelper$Dart.createObjectNotClosureException$member = function(){
  return ObjectNotClosureException$Dart.ObjectNotClosureException$$Factory();
}
;
ExceptionHelper$Dart.createObjectNotClosureException$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return ExceptionHelper$Dart.createObjectNotClosureException$member();
}
;
function native_ExceptionHelper_createObjectNotClosureException(){
  return ExceptionHelper$Dart.createObjectNotClosureException$member();
}

ExceptionHelper$Dart.createObjectNotClosureException$getter = function createObjectNotClosureException$getter(){
  return ExceptionHelper$Dart.createObjectNotClosureException$named;
}
;
ExceptionHelper$Dart.createNoSuchMethodException$member = function(receiver, functionName, arguments_0){
  return NoSuchMethodException$Dart.NoSuchMethodException$$Factory(receiver, functionName, arguments_0);
}
;
ExceptionHelper$Dart.createNoSuchMethodException$named = function($n, $o, receiver, functionName, arguments_0){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return ExceptionHelper$Dart.createNoSuchMethodException$member(receiver, functionName, arguments_0);
}
;
function native_ExceptionHelper_createNoSuchMethodException(receiver, functionName, arguments_0){
  return ExceptionHelper$Dart.createNoSuchMethodException$member(receiver, functionName, arguments_0);
}

ExceptionHelper$Dart.createNoSuchMethodException$getter = function createNoSuchMethodException$getter(){
  return ExceptionHelper$Dart.createNoSuchMethodException$named;
}
;
ExceptionHelper$Dart.createTypeError$member = function(srcType, dstType){
  return TypeError$Dart.TypeError$$Factory(srcType, dstType);
}
;
ExceptionHelper$Dart.createTypeError$named = function($n, $o, srcType, dstType){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return ExceptionHelper$Dart.createTypeError$member(srcType, dstType);
}
;
function native_ExceptionHelper_createTypeError(srcType, dstType){
  return ExceptionHelper$Dart.createTypeError$member(srcType, dstType);
}

ExceptionHelper$Dart.createTypeError$getter = function createTypeError$getter(){
  return ExceptionHelper$Dart.createTypeError$named;
}
;
ExceptionHelper$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
ExceptionHelper$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
ExceptionHelper$Dart.ExceptionHelper$$Factory = function(){
  var tmp$0 = new ExceptionHelper$Dart;
  tmp$0.$typeInfo = ExceptionHelper$Dart.$lookupRTT();
  ExceptionHelper$Dart.$Initializer.call(tmp$0);
  ExceptionHelper$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function _CoreJsUtil$Dart(){
}

_CoreJsUtil$Dart.$lookupRTT = function(){
  return RTT.create($cls('_CoreJsUtil$Dart'));
}
;
_CoreJsUtil$Dart.$addTo = function(target){
  var rtt = _CoreJsUtil$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
_CoreJsUtil$Dart.prototype.$implements$_CoreJsUtil$Dart = 1;
_CoreJsUtil$Dart.prototype.$implements$Object$Dart = 1;
_CoreJsUtil$Dart._newMapLiteral$$member_ = function(){
  return LinkedHashMapImplementation$Dart.LinkedHashMapImplementation$$Factory(LinkedHashMapImplementation$Dart.$lookupRTT());
}
;
_CoreJsUtil$Dart._newMapLiteral$$named_ = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return _CoreJsUtil$Dart._newMapLiteral$$member_();
}
;
function native__CoreJsUtil__newMapLiteral(){
  return _CoreJsUtil$Dart._newMapLiteral$$member_();
}

_CoreJsUtil$Dart._newMapLiteral$$getter_ = function _newMapLiteral$$getter_(){
  return _CoreJsUtil$Dart._newMapLiteral$$named_;
}
;
_CoreJsUtil$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
_CoreJsUtil$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
_CoreJsUtil$Dart._CoreJsUtil$$Factory = function(){
  var tmp$0 = new _CoreJsUtil$Dart;
  tmp$0.$typeInfo = _CoreJsUtil$Dart.$lookupRTT();
  _CoreJsUtil$Dart.$Initializer.call(tmp$0);
  _CoreJsUtil$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function DateImplementation$Dart(){
}

DateImplementation$Dart.$lookupRTT = function(){
  return RTT.create($cls('DateImplementation$Dart'), DateImplementation$Dart.$RTTimplements);
}
;
DateImplementation$Dart.$RTTimplements = function(rtt){
  DateImplementation$Dart.$addTo(rtt);
}
;
DateImplementation$Dart.$addTo = function(target){
  var rtt = DateImplementation$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Date$Dart.$addTo(target);
}
;
DateImplementation$Dart.prototype.$implements$DateImplementation$Dart = 1;
DateImplementation$Dart.prototype.$implements$Date$Dart = 1;
DateImplementation$Dart.prototype.$implements$Comparable$Dart = 1;
DateImplementation$Dart.prototype.$implements$Object$Dart = 1;
DateImplementation$Dart.DateImplementation$$Factory = function(years, month, day, hours, minutes, seconds, milliseconds){
  return DateImplementation$Dart.DateImplementation$withTimeZone$18$Factory(years, month, day, hours, minutes, seconds, milliseconds, TimeZoneImplementation$Dart.TimeZoneImplementation$local$22$Factory());
}
;
DateImplementation$Dart.withTimeZone$Constructor = function(years, month, day, hours, minutes, seconds, milliseconds, timeZone){
  Object.$Constructor.call(this);
}
;
DateImplementation$Dart.withTimeZone$Initializer = function(years, month, day, hours, minutes, seconds, milliseconds, timeZone){
  Object.$Initializer.call(this);
  this.timeZone$field = timeZone;
  this.value$field = DateImplementation$Dart._valueFromDecomposed$$member_(years, month, day, hours, minutes, seconds, milliseconds, timeZone.isUtc$getter());
}
;
DateImplementation$Dart.DateImplementation$withTimeZone$18$Factory = function(years, month, day, hours, minutes, seconds, milliseconds, timeZone){
  var tmp$0 = new DateImplementation$Dart;
  tmp$0.$typeInfo = DateImplementation$Dart.$lookupRTT();
  DateImplementation$Dart.withTimeZone$Initializer.call(tmp$0, years, month, day, hours, minutes, seconds, milliseconds, timeZone);
  DateImplementation$Dart.withTimeZone$Constructor.call(tmp$0, years, month, day, hours, minutes, seconds, milliseconds, timeZone);
  return tmp$0;
}
;
DateImplementation$Dart.now$Constructor = function(){
  Object.$Constructor.call(this);
}
;
DateImplementation$Dart.now$Initializer = function(){
  Object.$Initializer.call(this);
  this.timeZone$field = TimeZoneImplementation$Dart.TimeZoneImplementation$local$22$Factory();
  this.value$field = DateImplementation$Dart._now$$member_();
}
;
DateImplementation$Dart.DateImplementation$now$18$Factory = function(){
  var tmp$0 = new DateImplementation$Dart;
  tmp$0.$typeInfo = DateImplementation$Dart.$lookupRTT();
  DateImplementation$Dart.now$Initializer.call(tmp$0);
  DateImplementation$Dart.now$Constructor.call(tmp$0);
  return tmp$0;
}
;
DateImplementation$Dart.fromString$Constructor = function(formattedString){
  Object.$Constructor.call(this);
}
;
DateImplementation$Dart.fromString$Initializer = function(formattedString){
  Object.$Initializer.call(this);
  this.timeZone$field = TimeZoneImplementation$Dart.TimeZoneImplementation$local$22$Factory();
  this.value$field = DateImplementation$Dart._valueFromString$$member_(formattedString);
}
;
DateImplementation$Dart.DateImplementation$fromString$18$Factory = function(formattedString){
  var tmp$0 = new DateImplementation$Dart;
  tmp$0.$typeInfo = DateImplementation$Dart.$lookupRTT();
  DateImplementation$Dart.fromString$Initializer.call(tmp$0, formattedString);
  DateImplementation$Dart.fromString$Constructor.call(tmp$0, formattedString);
  return tmp$0;
}
;
DateImplementation$Dart.fromEpoch$Constructor = function(value, timeZone){
  Object.$Constructor.call(this);
}
;
DateImplementation$Dart.fromEpoch$Initializer = function(value, timeZone){
  Object.$Initializer.call(this);
  this.value$field = value;
  this.timeZone$field = timeZone;
}
;
DateImplementation$Dart.DateImplementation$fromEpoch$18$Factory = function(value, timeZone){
  var tmp$0 = new DateImplementation$Dart;
  tmp$0.$typeInfo = DateImplementation$Dart.$lookupRTT();
  DateImplementation$Dart.fromEpoch$Initializer.call(tmp$0, value, timeZone);
  DateImplementation$Dart.fromEpoch$Constructor.call(tmp$0, value, timeZone);
  return tmp$0;
}
;
DateImplementation$Dart.prototype.EQ$operator = function(other){
  var tmp$0;
  if (!!!(tmp$0 = other , tmp$0 != null && tmp$0.$implements$DateImplementation$Dart)) {
    return false;
  }
  return EQ$operator(this.value$getter(), other.value$getter()) && EQ$operator(this.timeZone$getter(), other.timeZone$getter());
}
;
DateImplementation$Dart.prototype.compareTo$member = function(other){
  return this.value$getter().compareTo$named(1, $noargs, other.value$getter());
}
;
DateImplementation$Dart.prototype.compareTo$named = function($n, $o, other){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DateImplementation$Dart.prototype.compareTo$member.call(this, other);
}
;
DateImplementation$Dart.prototype.compareTo$getter = function compareTo$getter(){
  return $bind(DateImplementation$Dart.prototype.compareTo$named, this);
}
;
DateImplementation$Dart.prototype.changeTimeZone$member = function(targetTimeZone){
  if (EQ$operator(targetTimeZone, $Dart$Null)) {
    targetTimeZone = TimeZoneImplementation$Dart.TimeZoneImplementation$local$22$Factory();
  }
  return DateImplementation$Dart.DateImplementation$fromEpoch$18$Factory(this.value$getter(), targetTimeZone);
}
;
DateImplementation$Dart.prototype.changeTimeZone$named = function($n, $o, targetTimeZone){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DateImplementation$Dart.prototype.changeTimeZone$member.call(this, targetTimeZone);
}
;
DateImplementation$Dart.prototype.changeTimeZone$getter = function changeTimeZone$getter(){
  return $bind(DateImplementation$Dart.prototype.changeTimeZone$named, this);
}
;
DateImplementation$Dart.prototype.year$named = function(){
  return this.year$getter().apply(this, arguments);
}
;
DateImplementation$Dart.prototype.year$getter = function(){
  return this._getYear$$member_(this.value$getter(), this.isUtc$member());
}
;
DateImplementation$Dart.prototype.month$named = function(){
  return this.month$getter().apply(this, arguments);
}
;
DateImplementation$Dart.prototype.month$getter = function(){
  return this._getMonth$$member_(this.value$getter(), this.isUtc$member());
}
;
DateImplementation$Dart.prototype.day$named = function(){
  return this.day$getter().apply(this, arguments);
}
;
DateImplementation$Dart.prototype.day$getter = function(){
  return this._getDay$$member_(this.value$getter(), this.isUtc$member());
}
;
DateImplementation$Dart.prototype.hours$named = function(){
  return this.hours$getter().apply(this, arguments);
}
;
DateImplementation$Dart.prototype.hours$getter = function(){
  return this._getHours$$member_(this.value$getter(), this.isUtc$member());
}
;
DateImplementation$Dart.prototype.minutes$named = function(){
  return this.minutes$getter().apply(this, arguments);
}
;
DateImplementation$Dart.prototype.minutes$getter = function(){
  return this._getMinutes$$member_(this.value$getter(), this.isUtc$member());
}
;
DateImplementation$Dart.prototype.seconds$named = function(){
  return this.seconds$getter().apply(this, arguments);
}
;
DateImplementation$Dart.prototype.seconds$getter = function(){
  return this._getSeconds$$member_(this.value$getter(), this.isUtc$member());
}
;
DateImplementation$Dart.prototype.milliseconds$named = function(){
  return this.milliseconds$getter().apply(this, arguments);
}
;
DateImplementation$Dart.prototype.milliseconds$getter = function(){
  return this._getMilliseconds$$member_(this.value$getter(), this.isUtc$member());
}
;
DateImplementation$Dart.prototype.weekday$named = function(){
  return this.weekday$getter().apply(this, arguments);
}
;
DateImplementation$Dart.prototype.weekday$getter = function(){
  var unixTimeStart = DateImplementation$Dart.DateImplementation$withTimeZone$18$Factory(1970, 1, 1, 0, 0, 0, 0, this.timeZone$getter());
  var msSince1970 = this.difference$named(1, $noargs, unixTimeStart).inMilliseconds$getter();
  if (LT$operator(this.hours$getter(), 2)) {
    msSince1970 = ADD$operator(msSince1970, MUL$operator(2, Duration$Dart.MILLISECONDS_PER_HOUR$getter()));
  }
  var daysSince1970 = DIV$operator(msSince1970, Duration$Dart.MILLISECONDS_PER_DAY$getter()).floor$named(0, $noargs).toInt$named(0, $noargs);
  return MOD$operator(ADD$operator(daysSince1970, Date$Dart.THU$getter()), Date$Dart.DAYS_IN_WEEK$getter());
}
;
DateImplementation$Dart.prototype.isLocalTime$member = function(){
  return !this.timeZone$getter().isUtc$getter();
}
;
DateImplementation$Dart.prototype.isLocalTime$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DateImplementation$Dart.prototype.isLocalTime$member.call(this);
}
;
DateImplementation$Dart.prototype.isLocalTime$getter = function isLocalTime$getter(){
  return $bind(DateImplementation$Dart.prototype.isLocalTime$named, this);
}
;
DateImplementation$Dart.prototype.isUtc$member = function(){
  return this.timeZone$getter().isUtc$getter();
}
;
DateImplementation$Dart.prototype.isUtc$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DateImplementation$Dart.prototype.isUtc$member.call(this);
}
;
DateImplementation$Dart.prototype.isUtc$getter = function isUtc$getter(){
  return $bind(DateImplementation$Dart.prototype.isUtc$named, this);
}
;
function DateImplementation$Dart$toString$c0$threeDigits$23_8_2$Hoisted(n){
  if (GTE$operator(n, 100)) {
    return '' + $toString(n) + '';
  }
  if (GT$operator(n, 10)) {
    return '0' + $toString(n) + '';
  }
  return '00' + $toString(n) + '';
}

function DateImplementation$Dart$toString$c0$threeDigits$23_8_2$Hoisted$named($n, $o, n){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DateImplementation$Dart$toString$c0$threeDigits$23_8_2$Hoisted(n);
}

function DateImplementation$Dart$toString$c1$twoDigits$23_8_2$Hoisted(n){
  if (GTE$operator(n, 10)) {
    return '' + $toString(n) + '';
  }
  return '0' + $toString(n) + '';
}

function DateImplementation$Dart$toString$c1$twoDigits$23_8_2$Hoisted$named($n, $o, n){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DateImplementation$Dart$toString$c1$twoDigits$23_8_2$Hoisted(n);
}

DateImplementation$Dart.prototype.toString$member = function(){
  var threeDigits = $bind(DateImplementation$Dart$toString$c0$threeDigits$23_8_2$Hoisted$named, $Dart$Null);
  var twoDigits = $bind(DateImplementation$Dart$toString$c1$twoDigits$23_8_2$Hoisted$named, $Dart$Null);
  var m = twoDigits(1, $noargs, this.month$getter());
  var d = twoDigits(1, $noargs, this.day$getter());
  var h = twoDigits(1, $noargs, this.hours$getter());
  var min = twoDigits(1, $noargs, this.minutes$getter());
  var sec = twoDigits(1, $noargs, this.seconds$getter());
  var ms = threeDigits(1, $noargs, this.milliseconds$getter());
  if (this.timeZone$getter().isUtc$getter()) {
    return '' + $toString(this.year$getter()) + '-' + $toString(m) + '-' + $toString(d) + ' ' + $toString(h) + ':' + $toString(min) + ':' + $toString(sec) + '.' + $toString(ms) + 'Z';
  }
   else {
    return '' + $toString(this.year$getter()) + '-' + $toString(m) + '-' + $toString(d) + ' ' + $toString(h) + ':' + $toString(min) + ':' + $toString(sec) + '.' + $toString(ms) + '';
  }
}
;
DateImplementation$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DateImplementation$Dart.prototype.toString$member.call(this);
}
;
DateImplementation$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(DateImplementation$Dart.prototype.toString$named, this);
}
;
DateImplementation$Dart.prototype.add$member = function(duration){
  return DateImplementation$Dart.DateImplementation$fromEpoch$18$Factory(ADD$operator(this.value$getter(), duration.inMilliseconds$getter()), this.timeZone$getter());
}
;
DateImplementation$Dart.prototype.add$named = function($n, $o, duration){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DateImplementation$Dart.prototype.add$member.call(this, duration);
}
;
DateImplementation$Dart.prototype.add$getter = function add$getter(){
  return $bind(DateImplementation$Dart.prototype.add$named, this);
}
;
DateImplementation$Dart.prototype.subtract$member = function(duration){
  return DateImplementation$Dart.DateImplementation$fromEpoch$18$Factory(SUB$operator(this.value$getter(), duration.inMilliseconds$getter()), this.timeZone$getter());
}
;
DateImplementation$Dart.prototype.subtract$named = function($n, $o, duration){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DateImplementation$Dart.prototype.subtract$member.call(this, duration);
}
;
DateImplementation$Dart.prototype.subtract$getter = function subtract$getter(){
  return $bind(DateImplementation$Dart.prototype.subtract$named, this);
}
;
DateImplementation$Dart.prototype.difference$member = function(other){
  return DurationImplementation$Dart.DurationImplementation$$Factory(0, 0, 0, 0, SUB$operator(this.value$getter(), other.value$getter()));
}
;
DateImplementation$Dart.prototype.difference$named = function($n, $o, other){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DateImplementation$Dart.prototype.difference$member.call(this, other);
}
;
DateImplementation$Dart.prototype.difference$getter = function difference$getter(){
  return $bind(DateImplementation$Dart.prototype.difference$named, this);
}
;
DateImplementation$Dart.prototype.value$named = function(){
  return this.value$getter().apply(this, arguments);
}
;
DateImplementation$Dart.prototype.value$getter = function(){
  return this.value$field;
}
;
DateImplementation$Dart.prototype.timeZone$named = function(){
  return this.timeZone$getter().apply(this, arguments);
}
;
DateImplementation$Dart.prototype.timeZone$getter = function(){
  return this.timeZone$field;
}
;
DateImplementation$Dart._valueFromDecomposed$$member_ = function(years, month, day, hours, minutes, seconds, milliseconds, isUtc){
  return native_DateImplementation__valueFromDecomposed(years, month, day, hours, minutes, seconds, milliseconds, isUtc);
}
;
DateImplementation$Dart._valueFromDecomposed$$named_ = function($n, $o, years, month, day, hours, minutes, seconds, milliseconds, isUtc){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 8)
    $nsme();
  return DateImplementation$Dart._valueFromDecomposed$$member_(years, month, day, hours, minutes, seconds, milliseconds, isUtc);
}
;
DateImplementation$Dart._valueFromDecomposed$$getter_ = function _valueFromDecomposed$$getter_(){
  return DateImplementation$Dart._valueFromDecomposed$$named_;
}
;
DateImplementation$Dart._valueFromString$$member_ = function(str){
  return native_DateImplementation__valueFromString(str);
}
;
DateImplementation$Dart._valueFromString$$named_ = function($n, $o, str){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DateImplementation$Dart._valueFromString$$member_(str);
}
;
DateImplementation$Dart._valueFromString$$getter_ = function _valueFromString$$getter_(){
  return DateImplementation$Dart._valueFromString$$named_;
}
;
DateImplementation$Dart._now$$member_ = function(){
  return native_DateImplementation__now();
}
;
DateImplementation$Dart._now$$named_ = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DateImplementation$Dart._now$$member_();
}
;
DateImplementation$Dart._now$$getter_ = function _now$$getter_(){
  return DateImplementation$Dart._now$$named_;
}
;
DateImplementation$Dart.prototype._getYear$$member_ = function(value, isUtc){
  return native_DateImplementation__getYear.call(this, value, isUtc);
}
;
DateImplementation$Dart.prototype._getYear$$named_ = function($n, $o, value, isUtc){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return DateImplementation$Dart.prototype._getYear$$member_.call(this, value, isUtc);
}
;
DateImplementation$Dart.prototype._getYear$$getter_ = function _getYear$$getter_(){
  return $bind(DateImplementation$Dart.prototype._getYear$$named_, this);
}
;
DateImplementation$Dart.prototype._getMonth$$member_ = function(value, isUtc){
  return native_DateImplementation__getMonth.call(this, value, isUtc);
}
;
DateImplementation$Dart.prototype._getMonth$$named_ = function($n, $o, value, isUtc){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return DateImplementation$Dart.prototype._getMonth$$member_.call(this, value, isUtc);
}
;
DateImplementation$Dart.prototype._getMonth$$getter_ = function _getMonth$$getter_(){
  return $bind(DateImplementation$Dart.prototype._getMonth$$named_, this);
}
;
DateImplementation$Dart.prototype._getDay$$member_ = function(value, isUtc){
  return native_DateImplementation__getDay.call(this, value, isUtc);
}
;
DateImplementation$Dart.prototype._getDay$$named_ = function($n, $o, value, isUtc){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return DateImplementation$Dart.prototype._getDay$$member_.call(this, value, isUtc);
}
;
DateImplementation$Dart.prototype._getDay$$getter_ = function _getDay$$getter_(){
  return $bind(DateImplementation$Dart.prototype._getDay$$named_, this);
}
;
DateImplementation$Dart.prototype._getHours$$member_ = function(value, isUtc){
  return native_DateImplementation__getHours.call(this, value, isUtc);
}
;
DateImplementation$Dart.prototype._getHours$$named_ = function($n, $o, value, isUtc){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return DateImplementation$Dart.prototype._getHours$$member_.call(this, value, isUtc);
}
;
DateImplementation$Dart.prototype._getHours$$getter_ = function _getHours$$getter_(){
  return $bind(DateImplementation$Dart.prototype._getHours$$named_, this);
}
;
DateImplementation$Dart.prototype._getMinutes$$member_ = function(value, isUtc){
  return native_DateImplementation__getMinutes.call(this, value, isUtc);
}
;
DateImplementation$Dart.prototype._getMinutes$$named_ = function($n, $o, value, isUtc){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return DateImplementation$Dart.prototype._getMinutes$$member_.call(this, value, isUtc);
}
;
DateImplementation$Dart.prototype._getMinutes$$getter_ = function _getMinutes$$getter_(){
  return $bind(DateImplementation$Dart.prototype._getMinutes$$named_, this);
}
;
DateImplementation$Dart.prototype._getSeconds$$member_ = function(value, isUtc){
  return native_DateImplementation__getSeconds.call(this, value, isUtc);
}
;
DateImplementation$Dart.prototype._getSeconds$$named_ = function($n, $o, value, isUtc){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return DateImplementation$Dart.prototype._getSeconds$$member_.call(this, value, isUtc);
}
;
DateImplementation$Dart.prototype._getSeconds$$getter_ = function _getSeconds$$getter_(){
  return $bind(DateImplementation$Dart.prototype._getSeconds$$named_, this);
}
;
DateImplementation$Dart.prototype._getMilliseconds$$member_ = function(value, isUtc){
  return native_DateImplementation__getMilliseconds.call(this, value, isUtc);
}
;
DateImplementation$Dart.prototype._getMilliseconds$$named_ = function($n, $o, value, isUtc){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return DateImplementation$Dart.prototype._getMilliseconds$$member_.call(this, value, isUtc);
}
;
DateImplementation$Dart.prototype._getMilliseconds$$getter_ = function _getMilliseconds$$getter_(){
  return $bind(DateImplementation$Dart.prototype._getMilliseconds$$named_, this);
}
;
DateImplementation$Dart.prototype.$const_id = function(){
  return $cls('DateImplementation$Dart') + (':' + $dart_const_id(this.year$field)) + (':' + $dart_const_id(this.month$field)) + (':' + $dart_const_id(this.day$field)) + (':' + $dart_const_id(this.hours$field)) + (':' + $dart_const_id(this.minutes$field)) + (':' + $dart_const_id(this.seconds$field)) + (':' + $dart_const_id(this.milliseconds$field)) + (':' + $dart_const_id(this.weekday$field)) + (':' + $dart_const_id(this.value$field)) + (':' + $dart_const_id(this.timeZone$field));
}
;
function SendPortImpl$Dart(){
}

SendPortImpl$Dart.$lookupRTT = function(){
  return RTT.create($cls('SendPortImpl$Dart'), SendPortImpl$Dart.$RTTimplements);
}
;
SendPortImpl$Dart.$RTTimplements = function(rtt){
  SendPortImpl$Dart.$addTo(rtt);
}
;
SendPortImpl$Dart.$addTo = function(target){
  var rtt = SendPortImpl$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  SendPort$Dart.$addTo(target);
}
;
SendPortImpl$Dart.prototype.$implements$SendPortImpl$Dart = 1;
SendPortImpl$Dart.prototype.$implements$SendPort$Dart = 1;
SendPortImpl$Dart.prototype.$implements$Hashable$Dart = 1;
SendPortImpl$Dart.prototype.$implements$Object$Dart = 1;
SendPortImpl$Dart.$Constructor = function(_workerId, _isolateId, _receivePortId){
  Object.$Constructor.call(this);
}
;
SendPortImpl$Dart.$Initializer = function(_workerId, _isolateId, _receivePortId){
  Object.$Initializer.call(this);
  this._workerId$$field_ = _workerId;
  this._isolateId$$field_ = _isolateId;
  this._receivePortId$$field_ = _receivePortId;
}
;
SendPortImpl$Dart.SendPortImpl$$Factory = function(_workerId, _isolateId, _receivePortId){
  var tmp$0 = new SendPortImpl$Dart;
  tmp$0.$typeInfo = SendPortImpl$Dart.$lookupRTT();
  SendPortImpl$Dart.$Initializer.call(tmp$0, _workerId, _isolateId, _receivePortId);
  SendPortImpl$Dart.$Constructor.call(tmp$0, _workerId, _isolateId, _receivePortId);
  return tmp$0;
}
;
SendPortImpl$Dart.prototype.send$member = function(message, replyTo){
  if (PromiseQueue$Dart.isEmpty$member()) {
    this._sendNow$$named_(2, $noargs, message, replyTo);
  }
   else {
    this._enqueueSend$$member_(message, replyTo);
  }
}
;
SendPortImpl$Dart.prototype.send$named = function($n, $o, message, replyTo){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 1:
      replyTo = $o.replyTo?(++seen , $o.replyTo):(++def , $Dart$Null);
  }
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return SendPortImpl$Dart.prototype.send$member.call(this, message, replyTo);
}
;
SendPortImpl$Dart.prototype.send$getter = function send$getter(){
  return $bind(SendPortImpl$Dart.prototype.send$named, this);
}
;
function SendPortImpl$Dart$_enqueueSend$c0$17_17$Hoisted(dartc_scp$0, ignored){
  this._sendNow$$named_(2, $noargs, dartc_scp$0.message, dartc_scp$0.replyTo);
}

function SendPortImpl$Dart$_enqueueSend$c0$17_17$Hoisted$named($s0, $n, $o, ignored){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return SendPortImpl$Dart$_enqueueSend$c0$17_17$Hoisted.call(this, $s0, ignored);
}

SendPortImpl$Dart.prototype._enqueueSend$$member_ = function(message, replyTo){
  var dartc_scp$0 = {message:message, replyTo:replyTo};
  PromiseQueue$Dart.enqueue$member($intern(RTT.setTypeInfo([], Array.$lookupRTT()), [''])).then$named(1, $noargs, $bind(SendPortImpl$Dart$_enqueueSend$c0$17_17$Hoisted$named, this, dartc_scp$0));
}
;
SendPortImpl$Dart.prototype._enqueueSend$$named_ = function($n, $o, message, replyTo){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return SendPortImpl$Dart.prototype._enqueueSend$$member_.call(this, message, replyTo);
}
;
SendPortImpl$Dart.prototype._enqueueSend$$getter_ = function _enqueueSend$$getter_(){
  return $bind(SendPortImpl$Dart.prototype._enqueueSend$$named_, this);
}
;
SendPortImpl$Dart.prototype._sendNow$$member_ = function(message, replyTo){
  return native_SendPortImpl__sendNow.call(this, message, replyTo);
}
;
SendPortImpl$Dart.prototype._sendNow$$named_ = function($n, $o, message, replyTo){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return SendPortImpl$Dart.prototype._sendNow$$member_.call(this, message, replyTo);
}
;
SendPortImpl$Dart.prototype._sendNow$$getter_ = function _sendNow$$getter_(){
  return $bind(SendPortImpl$Dart.prototype._sendNow$$named_, this);
}
;
SendPortImpl$Dart.prototype.call$member = function(message){
  var result = ReceivePortSingleShotImpl$Dart.ReceivePortSingleShotImpl$$Factory();
  this.send$named(2, $noargs, message, result.toSendPort$named(0, $noargs));
  return result;
}
;
SendPortImpl$Dart.prototype.call$named = function($n, $o, message){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return SendPortImpl$Dart.prototype.call$member.call(this, message);
}
;
SendPortImpl$Dart.prototype.call$getter = function call$getter(){
  return $bind(SendPortImpl$Dart.prototype.call$named, this);
}
;
SendPortImpl$Dart.prototype._callNow$$member_ = function(message){
  var result = ReceivePortSingleShotImpl$Dart.ReceivePortSingleShotImpl$$Factory();
  this._sendNow$$named_(2, $noargs, message, result.toSendPort$named(0, $noargs));
  return result;
}
;
SendPortImpl$Dart.prototype._callNow$$named_ = function($n, $o, message){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return SendPortImpl$Dart.prototype._callNow$$member_.call(this, message);
}
;
SendPortImpl$Dart.prototype._callNow$$getter_ = function _callNow$$getter_(){
  return $bind(SendPortImpl$Dart.prototype._callNow$$named_, this);
}
;
SendPortImpl$Dart.prototype.EQ$operator = function(other){
  var tmp$0;
  return !!(tmp$0 = other , tmp$0 != null && tmp$0.$implements$SendPortImpl$Dart) && EQ$operator(this._workerId$$getter_(), other._workerId$$getter_()) && EQ$operator(this._isolateId$$getter_(), other._isolateId$$getter_()) && EQ$operator(this._receivePortId$$getter_(), other._receivePortId$$getter_());
}
;
SendPortImpl$Dart.prototype.hashCode$member = function(){
  return BIT_XOR$operator(BIT_XOR$operator(SHL$operator(this._workerId$$getter_(), 16), SHL$operator(this._isolateId$$getter_(), 8)), this._receivePortId$$getter_());
}
;
SendPortImpl$Dart.prototype.hashCode$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return SendPortImpl$Dart.prototype.hashCode$member.call(this);
}
;
SendPortImpl$Dart.prototype.hashCode$getter = function hashCode$getter(){
  return $bind(SendPortImpl$Dart.prototype.hashCode$named, this);
}
;
SendPortImpl$Dart.prototype._receivePortId$$named_ = function(){
  return this._receivePortId$$getter_().apply(this, arguments);
}
;
SendPortImpl$Dart.prototype._receivePortId$$getter_ = function(){
  return this._receivePortId$$field_;
}
;
SendPortImpl$Dart.prototype._isolateId$$named_ = function(){
  return this._isolateId$$getter_().apply(this, arguments);
}
;
SendPortImpl$Dart.prototype._isolateId$$getter_ = function(){
  return this._isolateId$$field_;
}
;
SendPortImpl$Dart.prototype._workerId$$named_ = function(){
  return this._workerId$$getter_().apply(this, arguments);
}
;
SendPortImpl$Dart.prototype._workerId$$getter_ = function(){
  return this._workerId$$field_;
}
;
SendPortImpl$Dart._create$$member_ = function(workerId, isolateId, receivePortId){
  return SendPortImpl$Dart.SendPortImpl$$Factory(workerId, isolateId, receivePortId);
}
;
SendPortImpl$Dart._create$$named_ = function($n, $o, workerId, isolateId, receivePortId){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return SendPortImpl$Dart._create$$member_(workerId, isolateId, receivePortId);
}
;
function native_SendPortImpl__create(workerId, isolateId, receivePortId){
  return SendPortImpl$Dart._create$$member_(workerId, isolateId, receivePortId);
}

SendPortImpl$Dart._create$$getter_ = function _create$$getter_(){
  return SendPortImpl$Dart._create$$named_;
}
;
SendPortImpl$Dart._getReceivePortId$$member_ = function(port){
  return port._receivePortId$$getter_();
}
;
SendPortImpl$Dart._getReceivePortId$$named_ = function($n, $o, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return SendPortImpl$Dart._getReceivePortId$$member_(port);
}
;
function native_SendPortImpl__getReceivePortId(port){
  return SendPortImpl$Dart._getReceivePortId$$member_(port);
}

SendPortImpl$Dart._getReceivePortId$$getter_ = function _getReceivePortId$$getter_(){
  return SendPortImpl$Dart._getReceivePortId$$named_;
}
;
SendPortImpl$Dart._getIsolateId$$member_ = function(port){
  return port._isolateId$$getter_();
}
;
SendPortImpl$Dart._getIsolateId$$named_ = function($n, $o, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return SendPortImpl$Dart._getIsolateId$$member_(port);
}
;
function native_SendPortImpl__getIsolateId(port){
  return SendPortImpl$Dart._getIsolateId$$member_(port);
}

SendPortImpl$Dart._getIsolateId$$getter_ = function _getIsolateId$$getter_(){
  return SendPortImpl$Dart._getIsolateId$$named_;
}
;
SendPortImpl$Dart._getWorkerId$$member_ = function(port){
  return port._workerId$$getter_();
}
;
SendPortImpl$Dart._getWorkerId$$named_ = function($n, $o, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return SendPortImpl$Dart._getWorkerId$$member_(port);
}
;
function native_SendPortImpl__getWorkerId(port){
  return SendPortImpl$Dart._getWorkerId$$member_(port);
}

SendPortImpl$Dart._getWorkerId$$getter_ = function _getWorkerId$$getter_(){
  return SendPortImpl$Dart._getWorkerId$$named_;
}
;
SendPortImpl$Dart.prototype.$const_id = function(){
  return $cls('SendPortImpl$Dart') + (':' + $dart_const_id(this._receivePortId$$field_)) + (':' + $dart_const_id(this._isolateId$$field_)) + (':' + $dart_const_id(this._workerId$$field_));
}
;
function ReceivePortFactory$Dart(){
}

ReceivePortFactory$Dart.$lookupRTT = function(){
  return RTT.create($cls('ReceivePortFactory$Dart'));
}
;
ReceivePortFactory$Dart.$addTo = function(target){
  var rtt = ReceivePortFactory$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
ReceivePortFactory$Dart.prototype.$implements$ReceivePortFactory$Dart = 1;
ReceivePortFactory$Dart.prototype.$implements$Object$Dart = 1;
ReceivePortFactory$Dart.ReceivePort$$Factory = function(){
  return ReceivePortImpl$Dart.ReceivePortImpl$$Factory();
}
;
ReceivePortFactory$Dart.ReceivePort$singleShot$11$Factory = function(){
  return ReceivePortSingleShotImpl$Dart.ReceivePortSingleShotImpl$$Factory();
}
;
function ReceivePortImpl$Dart(){
}

ReceivePortImpl$Dart.$lookupRTT = function(){
  return RTT.create($cls('ReceivePortImpl$Dart'), ReceivePortImpl$Dart.$RTTimplements);
}
;
ReceivePortImpl$Dart.$RTTimplements = function(rtt){
  ReceivePortImpl$Dart.$addTo(rtt);
}
;
ReceivePortImpl$Dart.$addTo = function(target){
  var rtt = ReceivePortImpl$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  ReceivePort$Dart.$addTo(target);
}
;
ReceivePortImpl$Dart.prototype.$implements$ReceivePortImpl$Dart = 1;
ReceivePortImpl$Dart.prototype.$implements$ReceivePort$Dart = 1;
ReceivePortImpl$Dart.prototype.$implements$Object$Dart = 1;
ReceivePortImpl$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
  this._register$$member_(this._id$$getter_());
}
;
ReceivePortImpl$Dart.$Initializer = function(){
  var tmp$1, tmp$0;
  Object.$Initializer.call(this);
  this._callback$$field_ = $Dart$Null;
  this._id$$field_ = (tmp$0 = ReceivePortImpl$Dart._nextFreeId$$getter_() , (ReceivePortImpl$Dart._nextFreeId$$setter_(tmp$1 = ADD$operator(tmp$0, 1)) , tmp$1 , tmp$0));
}
;
ReceivePortImpl$Dart.ReceivePortImpl$$Factory = function(){
  var tmp$0 = new ReceivePortImpl$Dart;
  tmp$0.$typeInfo = ReceivePortImpl$Dart.$lookupRTT();
  ReceivePortImpl$Dart.$Initializer.call(tmp$0);
  ReceivePortImpl$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
ReceivePortImpl$Dart.prototype.receive$member = function(onMessage){
  var tmp$0;
  this._callback$$setter_(tmp$0 = onMessage) , tmp$0;
}
;
ReceivePortImpl$Dart.prototype.receive$named = function($n, $o, onMessage){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return ReceivePortImpl$Dart.prototype.receive$member.call(this, onMessage);
}
;
ReceivePortImpl$Dart.prototype.receive$getter = function receive$getter(){
  return $bind(ReceivePortImpl$Dart.prototype.receive$named, this);
}
;
ReceivePortImpl$Dart.prototype.close$member = function(){
  var tmp$0;
  this._callback$$setter_(tmp$0 = $Dart$Null) , tmp$0;
  this._unregister$$member_(this._id$$getter_());
}
;
ReceivePortImpl$Dart.prototype.close$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return ReceivePortImpl$Dart.prototype.close$member.call(this);
}
;
ReceivePortImpl$Dart.prototype.close$getter = function close$getter(){
  return $bind(ReceivePortImpl$Dart.prototype.close$named, this);
}
;
ReceivePortImpl$Dart.prototype.toSendPort$member = function(){
  return this._toNewSendPort$$member_();
}
;
ReceivePortImpl$Dart.prototype.toSendPort$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return ReceivePortImpl$Dart.prototype.toSendPort$member.call(this);
}
;
ReceivePortImpl$Dart.prototype.toSendPort$getter = function toSendPort$getter(){
  return $bind(ReceivePortImpl$Dart.prototype.toSendPort$named, this);
}
;
ReceivePortImpl$Dart.prototype._toNewSendPort$$member_ = function(){
  return SendPortImpl$Dart.SendPortImpl$$Factory(ReceivePortImpl$Dart._currentWorkerId$$member_(), ReceivePortImpl$Dart._currentIsolateId$$member_(), this._id$$getter_());
}
;
ReceivePortImpl$Dart.prototype._toNewSendPort$$named_ = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return ReceivePortImpl$Dart.prototype._toNewSendPort$$member_.call(this);
}
;
ReceivePortImpl$Dart.prototype._toNewSendPort$$getter_ = function _toNewSendPort$$getter_(){
  return $bind(ReceivePortImpl$Dart.prototype._toNewSendPort$$named_, this);
}
;
ReceivePortImpl$Dart.prototype._id$$named_ = function(){
  return this._id$$getter_().apply(this, arguments);
}
;
ReceivePortImpl$Dart.prototype._id$$getter_ = function(){
  return this._id$$field_;
}
;
ReceivePortImpl$Dart.prototype._id$$setter_ = function(tmp$0){
  this._id$$field_ = tmp$0;
}
;
ReceivePortImpl$Dart.prototype._callback$$named_ = function(){
  return this._callback$$getter_().apply(this, arguments);
}
;
ReceivePortImpl$Dart.prototype._callback$$getter_ = function(){
  var tmp$0 = this._callback$$field_;
  var tmp$1 = static$initializing;
  if (tmp$0 === tmp$1)
    throw 'circular initialization';
  if (tmp$0 !== static$uninitialized)
    return tmp$0;
  this._callback$$field_ = tmp$1;
  var tmp$2 = $Dart$Null;
  this._callback$$field_ = tmp$2;
  return tmp$2;
}
;
ReceivePortImpl$Dart.prototype._callback$$setter_ = function(tmp$0){
  this._callback$$field_ = tmp$0;
}
;
ReceivePortImpl$Dart._nextFreeId$$named_ = function(){
  return ReceivePortImpl$Dart._nextFreeId$$getter_().apply(this, arguments);
}
;
ReceivePortImpl$Dart._nextFreeId$$getter_ = function(){
  return isolate$current.ReceivePortImpl$Dart_nextFreeId$$field_;
}
;
ReceivePortImpl$Dart._nextFreeId$$setter_ = function(tmp$0){
  isolate$current.ReceivePortImpl$Dart_nextFreeId$$field_ = tmp$0;
}
;
ReceivePortImpl$Dart.prototype._register$$member_ = function(id){
  return native_ReceivePortImpl__register.call(this, id);
}
;
ReceivePortImpl$Dart.prototype._register$$named_ = function($n, $o, id){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return ReceivePortImpl$Dart.prototype._register$$member_.call(this, id);
}
;
ReceivePortImpl$Dart.prototype._register$$getter_ = function _register$$getter_(){
  return $bind(ReceivePortImpl$Dart.prototype._register$$named_, this);
}
;
ReceivePortImpl$Dart.prototype._unregister$$member_ = function(id){
  return native_ReceivePortImpl__unregister.call(this, id);
}
;
ReceivePortImpl$Dart.prototype._unregister$$named_ = function($n, $o, id){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return ReceivePortImpl$Dart.prototype._unregister$$member_.call(this, id);
}
;
ReceivePortImpl$Dart.prototype._unregister$$getter_ = function _unregister$$getter_(){
  return $bind(ReceivePortImpl$Dart.prototype._unregister$$named_, this);
}
;
ReceivePortImpl$Dart._currentWorkerId$$member_ = function(){
  return native_ReceivePortImpl__currentWorkerId();
}
;
ReceivePortImpl$Dart._currentWorkerId$$named_ = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return ReceivePortImpl$Dart._currentWorkerId$$member_();
}
;
ReceivePortImpl$Dart._currentWorkerId$$getter_ = function _currentWorkerId$$getter_(){
  return ReceivePortImpl$Dart._currentWorkerId$$named_;
}
;
ReceivePortImpl$Dart._currentIsolateId$$member_ = function(){
  return native_ReceivePortImpl__currentIsolateId();
}
;
ReceivePortImpl$Dart._currentIsolateId$$named_ = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return ReceivePortImpl$Dart._currentIsolateId$$member_();
}
;
ReceivePortImpl$Dart._currentIsolateId$$getter_ = function _currentIsolateId$$getter_(){
  return ReceivePortImpl$Dart._currentIsolateId$$named_;
}
;
ReceivePortImpl$Dart._invokeCallback$$member_ = function(port, message, replyTo){
  if (port._callback$$getter_() != null) {
    port._callback$$getter_()(2, $noargs, message, replyTo);
  }
}
;
ReceivePortImpl$Dart._invokeCallback$$named_ = function($n, $o, port, message, replyTo){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return ReceivePortImpl$Dart._invokeCallback$$member_(port, message, replyTo);
}
;
function native_ReceivePortImpl__invokeCallback(port, message, replyTo){
  return ReceivePortImpl$Dart._invokeCallback$$member_(port, message, replyTo);
}

ReceivePortImpl$Dart._invokeCallback$$getter_ = function _invokeCallback$$getter_(){
  return ReceivePortImpl$Dart._invokeCallback$$named_;
}
;
ReceivePortImpl$Dart._getId$$member_ = function(port){
  return port._id$$getter_();
}
;
ReceivePortImpl$Dart._getId$$named_ = function($n, $o, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return ReceivePortImpl$Dart._getId$$member_(port);
}
;
function native_ReceivePortImpl__getId(port){
  return ReceivePortImpl$Dart._getId$$member_(port);
}

ReceivePortImpl$Dart._getId$$getter_ = function _getId$$getter_(){
  return ReceivePortImpl$Dart._getId$$named_;
}
;
ReceivePortImpl$Dart._getCallback$$member_ = function(port){
  return port._callback$$getter_();
}
;
ReceivePortImpl$Dart._getCallback$$named_ = function($n, $o, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return ReceivePortImpl$Dart._getCallback$$member_(port);
}
;
function native_ReceivePortImpl__getCallback(port){
  return ReceivePortImpl$Dart._getCallback$$member_(port);
}

ReceivePortImpl$Dart._getCallback$$getter_ = function _getCallback$$getter_(){
  return ReceivePortImpl$Dart._getCallback$$named_;
}
;
function ReceivePortSingleShotImpl$Dart(){
}

ReceivePortSingleShotImpl$Dart.$lookupRTT = function(){
  return RTT.create($cls('ReceivePortSingleShotImpl$Dart'), ReceivePortSingleShotImpl$Dart.$RTTimplements);
}
;
ReceivePortSingleShotImpl$Dart.$RTTimplements = function(rtt){
  ReceivePortSingleShotImpl$Dart.$addTo(rtt);
}
;
ReceivePortSingleShotImpl$Dart.$addTo = function(target){
  var rtt = ReceivePortSingleShotImpl$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  ReceivePort$Dart.$addTo(target);
}
;
ReceivePortSingleShotImpl$Dart.prototype.$implements$ReceivePortSingleShotImpl$Dart = 1;
ReceivePortSingleShotImpl$Dart.prototype.$implements$ReceivePort$Dart = 1;
ReceivePortSingleShotImpl$Dart.prototype.$implements$Object$Dart = 1;
ReceivePortSingleShotImpl$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
ReceivePortSingleShotImpl$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
  this._port$$field_ = ReceivePortImpl$Dart.ReceivePortImpl$$Factory();
}
;
ReceivePortSingleShotImpl$Dart.ReceivePortSingleShotImpl$$Factory = function(){
  var tmp$0 = new ReceivePortSingleShotImpl$Dart;
  tmp$0.$typeInfo = ReceivePortSingleShotImpl$Dart.$lookupRTT();
  ReceivePortSingleShotImpl$Dart.$Initializer.call(tmp$0);
  ReceivePortSingleShotImpl$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function ReceivePortSingleShotImpl$Dart$receive$c0$30_30$Hoisted(dartc_scp$0, message_0, replyTo_0){
  this._port$$getter_().close$named(0, $noargs);
  dartc_scp$0.callback(2, $noargs, message_0, replyTo_0);
}

function ReceivePortSingleShotImpl$Dart$receive$c0$30_30$Hoisted$named($s0, $n, $o, message, replyTo){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return ReceivePortSingleShotImpl$Dart$receive$c0$30_30$Hoisted.call(this, $s0, message, replyTo);
}

ReceivePortSingleShotImpl$Dart.prototype.receive$member = function(callback){
  var dartc_scp$0 = {callback:callback};
  this._port$$getter_().receive$named(1, $noargs, $bind(ReceivePortSingleShotImpl$Dart$receive$c0$30_30$Hoisted$named, this, dartc_scp$0));
}
;
ReceivePortSingleShotImpl$Dart.prototype.receive$named = function($n, $o, callback){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return ReceivePortSingleShotImpl$Dart.prototype.receive$member.call(this, callback);
}
;
ReceivePortSingleShotImpl$Dart.prototype.receive$getter = function receive$getter(){
  return $bind(ReceivePortSingleShotImpl$Dart.prototype.receive$named, this);
}
;
ReceivePortSingleShotImpl$Dart.prototype.close$member = function(){
  this._port$$getter_().close$named(0, $noargs);
}
;
ReceivePortSingleShotImpl$Dart.prototype.close$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return ReceivePortSingleShotImpl$Dart.prototype.close$member.call(this);
}
;
ReceivePortSingleShotImpl$Dart.prototype.close$getter = function close$getter(){
  return $bind(ReceivePortSingleShotImpl$Dart.prototype.close$named, this);
}
;
ReceivePortSingleShotImpl$Dart.prototype.toSendPort$member = function(){
  return this._toNewSendPort$$member_();
}
;
ReceivePortSingleShotImpl$Dart.prototype.toSendPort$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return ReceivePortSingleShotImpl$Dart.prototype.toSendPort$member.call(this);
}
;
ReceivePortSingleShotImpl$Dart.prototype.toSendPort$getter = function toSendPort$getter(){
  return $bind(ReceivePortSingleShotImpl$Dart.prototype.toSendPort$named, this);
}
;
ReceivePortSingleShotImpl$Dart.prototype._toNewSendPort$$member_ = function(){
  return this._port$$getter_()._toNewSendPort$$named_(0, $noargs);
}
;
ReceivePortSingleShotImpl$Dart.prototype._toNewSendPort$$named_ = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return ReceivePortSingleShotImpl$Dart.prototype._toNewSendPort$$member_.call(this);
}
;
ReceivePortSingleShotImpl$Dart.prototype._toNewSendPort$$getter_ = function _toNewSendPort$$getter_(){
  return $bind(ReceivePortSingleShotImpl$Dart.prototype._toNewSendPort$$named_, this);
}
;
ReceivePortSingleShotImpl$Dart.prototype._port$$named_ = function(){
  return this._port$$getter_().apply(this, arguments);
}
;
ReceivePortSingleShotImpl$Dart.prototype._port$$getter_ = function(){
  return this._port$$field_;
}
;
function IsolateNatives$Dart(){
}

IsolateNatives$Dart.$lookupRTT = function(){
  return RTT.create($cls('IsolateNatives$Dart'));
}
;
IsolateNatives$Dart.$addTo = function(target){
  var rtt = IsolateNatives$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
IsolateNatives$Dart.prototype.$implements$IsolateNatives$Dart = 1;
IsolateNatives$Dart.prototype.$implements$Object$Dart = 1;
function IsolateNatives$Dart$spawn$c0$19_19$Hoisted(dartc_scp$1, msg, replyPort){
  assert(EQ$operator(msg, _SPAWNED_SIGNAL$$getter_()));
  dartc_scp$1.result.complete$named(1, $noargs, replyPort);
}

function IsolateNatives$Dart$spawn$c0$19_19$Hoisted$named($s0, $n, $o, msg, replyPort){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return IsolateNatives$Dart$spawn$c0$19_19$Hoisted($s0, msg, replyPort);
}

IsolateNatives$Dart.spawn$member = function(isolate, isLight){
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.result = PromiseImpl$Dart.PromiseImpl$$Factory(PromiseImpl$Dart.$lookupRTT([SendPort$Dart.$lookupRTT()]));
  var port = ReceivePortFactory$Dart.ReceivePort$singleShot$11$Factory();
  port.receive$named(1, $noargs, $bind(IsolateNatives$Dart$spawn$c0$19_19$Hoisted$named, $Dart$Null, dartc_scp$1));
  IsolateNatives$Dart._spawn$$member_(isolate, isLight, port.toSendPort$named(0, $noargs));
  return dartc_scp$1.result;
  dartc_scp$1 = $Dart$Null;
}
;
IsolateNatives$Dart.spawn$named = function($n, $o, isolate, isLight){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return IsolateNatives$Dart.spawn$member(isolate, isLight);
}
;
IsolateNatives$Dart.spawn$getter = function spawn$getter(){
  return IsolateNatives$Dart.spawn$named;
}
;
IsolateNatives$Dart._spawn$$member_ = function(isolate, light, port){
  return native_IsolateNatives__spawn(isolate, light, port);
}
;
IsolateNatives$Dart._spawn$$named_ = function($n, $o, isolate, light, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return IsolateNatives$Dart._spawn$$member_(isolate, light, port);
}
;
IsolateNatives$Dart._spawn$$getter_ = function _spawn$$getter_(){
  return IsolateNatives$Dart._spawn$$named_;
}
;
IsolateNatives$Dart.bind$member = function(f){
  return native_IsolateNatives_bind(f);
}
;
IsolateNatives$Dart.bind$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return IsolateNatives$Dart.bind$member(f);
}
;
IsolateNatives$Dart.bind$getter = function bind$getter(){
  return IsolateNatives$Dart.bind$named;
}
;
IsolateNatives$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
IsolateNatives$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
IsolateNatives$Dart.IsolateNatives$$Factory = function(){
  var tmp$0 = new IsolateNatives$Dart;
  tmp$0.$typeInfo = IsolateNatives$Dart.$lookupRTT();
  IsolateNatives$Dart.$Initializer.call(tmp$0);
  IsolateNatives$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function _IsolateJsUtil$Dart(){
}

_IsolateJsUtil$Dart.$lookupRTT = function(){
  return RTT.create($cls('_IsolateJsUtil$Dart'));
}
;
_IsolateJsUtil$Dart.$addTo = function(target){
  var rtt = _IsolateJsUtil$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
_IsolateJsUtil$Dart.prototype.$implements$_IsolateJsUtil$Dart = 1;
_IsolateJsUtil$Dart.prototype.$implements$Object$Dart = 1;
_IsolateJsUtil$Dart._promiseQueueProcess$$member_ = function(){
  PromiseQueue$Dart.process$member();
}
;
_IsolateJsUtil$Dart._promiseQueueProcess$$named_ = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return _IsolateJsUtil$Dart._promiseQueueProcess$$member_();
}
;
function native__IsolateJsUtil__promiseQueueProcess(){
  return _IsolateJsUtil$Dart._promiseQueueProcess$$member_();
}

_IsolateJsUtil$Dart._promiseQueueProcess$$getter_ = function _promiseQueueProcess$$getter_(){
  return _IsolateJsUtil$Dart._promiseQueueProcess$$named_;
}
;
_IsolateJsUtil$Dart._startIsolate$$member_ = function(isolate, replyTo){
  var port = ReceivePortFactory$Dart.ReceivePort$$Factory();
  replyTo.send$named(2, $noargs, _SPAWNED_SIGNAL$$getter_(), port.toSendPort$named(0, $noargs));
  isolate._run$$named_(1, $noargs, port);
}
;
_IsolateJsUtil$Dart._startIsolate$$named_ = function($n, $o, isolate, replyTo){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return _IsolateJsUtil$Dart._startIsolate$$member_(isolate, replyTo);
}
;
function native__IsolateJsUtil__startIsolate(isolate, replyTo){
  return _IsolateJsUtil$Dart._startIsolate$$member_(isolate, replyTo);
}

_IsolateJsUtil$Dart._startIsolate$$getter_ = function _startIsolate$$getter_(){
  return _IsolateJsUtil$Dart._startIsolate$$named_;
}
;
_IsolateJsUtil$Dart._toSendPort$$member_ = function(port){
  return port.toSendPort$named(0, $noargs);
}
;
_IsolateJsUtil$Dart._toSendPort$$named_ = function($n, $o, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _IsolateJsUtil$Dart._toSendPort$$member_(port);
}
;
function native__IsolateJsUtil__toSendPort(port){
  return _IsolateJsUtil$Dart._toSendPort$$member_(port);
}

_IsolateJsUtil$Dart._toSendPort$$getter_ = function _toSendPort$$getter_(){
  return _IsolateJsUtil$Dart._toSendPort$$named_;
}
;
_IsolateJsUtil$Dart._print$$member_ = function(msg){
  print$getter()(1, $noargs, msg);
}
;
_IsolateJsUtil$Dart._print$$named_ = function($n, $o, msg){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _IsolateJsUtil$Dart._print$$member_(msg);
}
;
function native__IsolateJsUtil__print(msg){
  return _IsolateJsUtil$Dart._print$$member_(msg);
}

_IsolateJsUtil$Dart._print$$getter_ = function _print$$getter_(){
  return _IsolateJsUtil$Dart._print$$named_;
}
;
_IsolateJsUtil$Dart._copyObject$$member_ = function(obj){
  return Copier$Dart.Copier$$Factory().traverse$named(1, $noargs, obj);
}
;
_IsolateJsUtil$Dart._copyObject$$named_ = function($n, $o, obj){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _IsolateJsUtil$Dart._copyObject$$member_(obj);
}
;
function native__IsolateJsUtil__copyObject(obj){
  return _IsolateJsUtil$Dart._copyObject$$member_(obj);
}

_IsolateJsUtil$Dart._copyObject$$getter_ = function _copyObject$$getter_(){
  return _IsolateJsUtil$Dart._copyObject$$named_;
}
;
_IsolateJsUtil$Dart._serializeObject$$member_ = function(obj){
  return Serializer$Dart.Serializer$$Factory().traverse$named(1, $noargs, obj);
}
;
_IsolateJsUtil$Dart._serializeObject$$named_ = function($n, $o, obj){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _IsolateJsUtil$Dart._serializeObject$$member_(obj);
}
;
function native__IsolateJsUtil__serializeObject(obj){
  return _IsolateJsUtil$Dart._serializeObject$$member_(obj);
}

_IsolateJsUtil$Dart._serializeObject$$getter_ = function _serializeObject$$getter_(){
  return _IsolateJsUtil$Dart._serializeObject$$named_;
}
;
_IsolateJsUtil$Dart._deserializeMessage$$member_ = function(message){
  return Deserializer$Dart.Deserializer$$Factory().deserialize$named(1, $noargs, message);
}
;
_IsolateJsUtil$Dart._deserializeMessage$$named_ = function($n, $o, message){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _IsolateJsUtil$Dart._deserializeMessage$$member_(message);
}
;
function native__IsolateJsUtil__deserializeMessage(message){
  return _IsolateJsUtil$Dart._deserializeMessage$$member_(message);
}

_IsolateJsUtil$Dart._deserializeMessage$$getter_ = function _deserializeMessage$$getter_(){
  return _IsolateJsUtil$Dart._deserializeMessage$$named_;
}
;
_IsolateJsUtil$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
_IsolateJsUtil$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
_IsolateJsUtil$Dart._IsolateJsUtil$$Factory = function(){
  var tmp$0 = new _IsolateJsUtil$Dart;
  tmp$0.$typeInfo = _IsolateJsUtil$Dart.$lookupRTT();
  _IsolateJsUtil$Dart.$Initializer.call(tmp$0);
  _IsolateJsUtil$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function _SPAWNED_SIGNAL$$getter_(){
  return 'spawned';
}

function MessageTraverser$Dart(){
}

MessageTraverser$Dart.$lookupRTT = function(){
  return RTT.create($cls('MessageTraverser$Dart'));
}
;
MessageTraverser$Dart.$addTo = function(target){
  var rtt = MessageTraverser$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
MessageTraverser$Dart.prototype.$implements$MessageTraverser$Dart = 1;
MessageTraverser$Dart.prototype.$implements$Object$Dart = 1;
MessageTraverser$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
MessageTraverser$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
MessageTraverser$Dart.MessageTraverser$$Factory = function(){
  var tmp$0 = new MessageTraverser$Dart;
  tmp$0.$typeInfo = MessageTraverser$Dart.$lookupRTT();
  MessageTraverser$Dart.$Initializer.call(tmp$0);
  MessageTraverser$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
MessageTraverser$Dart.isPrimitive$member = function(x){
  var tmp$0;
  return x == null || String.$instanceOf(x) || !!(tmp$0 = x , tmp$0 != null && tmp$0.$implements$num$Dart) || Boolean.$instanceOf(x);
}
;
MessageTraverser$Dart.isPrimitive$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MessageTraverser$Dart.isPrimitive$member(x);
}
;
MessageTraverser$Dart.isPrimitive$getter = function isPrimitive$getter(){
  return MessageTraverser$Dart.isPrimitive$named;
}
;
MessageTraverser$Dart.prototype.traverse$member = function(x){
  var tmp$0;
  if (MessageTraverser$Dart.isPrimitive$member(x)) {
    return this.visitPrimitive$member(x);
  }
  this._taggedObjects$$setter_(tmp$0 = ListFactory$Dart.List$$Factory(null, $Dart$Null)) , tmp$0;
  var result = $Dart$Null;
  try {
    result = this._dispatch$$member_(x);
  }
   finally {
    this._cleanup$$member_();
  }
  return result;
}
;
MessageTraverser$Dart.prototype.traverse$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MessageTraverser$Dart.prototype.traverse$member.call(this, x);
}
;
MessageTraverser$Dart.prototype.traverse$getter = function traverse$getter(){
  return $bind(MessageTraverser$Dart.prototype.traverse$named, this);
}
;
MessageTraverser$Dart.prototype._cleanup$$member_ = function(){
  var tmp$1, tmp$0;
  var len = this._taggedObjects$$getter_().length$getter();
  {
    var i = 0;
    for (; LT$operator(i, len); tmp$0 = i , (i = ADD$operator(tmp$0, 1) , tmp$0)) {
      this._clearAttachedInfo$$member_(this._taggedObjects$$getter_().INDEX$operator(i));
    }
  }
  this._taggedObjects$$setter_(tmp$1 = $Dart$Null) , tmp$1;
}
;
MessageTraverser$Dart.prototype._cleanup$$named_ = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return MessageTraverser$Dart.prototype._cleanup$$member_.call(this);
}
;
MessageTraverser$Dart.prototype._cleanup$$getter_ = function _cleanup$$getter_(){
  return $bind(MessageTraverser$Dart.prototype._cleanup$$named_, this);
}
;
MessageTraverser$Dart.prototype._attachInfo$$member_ = function(o, info){
  this._taggedObjects$$getter_().add$named(1, $noargs, o);
  this._setAttachedInfo$$member_(o, info);
}
;
MessageTraverser$Dart.prototype._attachInfo$$named_ = function($n, $o, o, info){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return MessageTraverser$Dart.prototype._attachInfo$$member_.call(this, o, info);
}
;
MessageTraverser$Dart.prototype._attachInfo$$getter_ = function _attachInfo$$getter_(){
  return $bind(MessageTraverser$Dart.prototype._attachInfo$$named_, this);
}
;
MessageTraverser$Dart.prototype._getInfo$$member_ = function(o){
  return this._getAttachedInfo$$member_(o);
}
;
MessageTraverser$Dart.prototype._getInfo$$named_ = function($n, $o, o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MessageTraverser$Dart.prototype._getInfo$$member_.call(this, o);
}
;
MessageTraverser$Dart.prototype._getInfo$$getter_ = function _getInfo$$getter_(){
  return $bind(MessageTraverser$Dart.prototype._getInfo$$named_, this);
}
;
MessageTraverser$Dart.prototype._dispatch$$member_ = function(x){
  var tmp$1, tmp$2, tmp$3, tmp$4, tmp$0;
  if (MessageTraverser$Dart.isPrimitive$member(x)) {
    return this.visitPrimitive$member(x);
  }
  if (!!(tmp$0 = x , tmp$0 != null && tmp$0.$implements$List$Dart)) {
    return this.visitList$member(x);
  }
  if (!!(tmp$1 = x , tmp$1 != null && tmp$1.$implements$Map$Dart)) {
    return this.visitMap$member(x);
  }
  if (!!(tmp$2 = x , tmp$2 != null && tmp$2.$implements$SendPortImpl$Dart)) {
    return this.visitSendPort$member(x);
  }
  if (!!(tmp$3 = x , tmp$3 != null && tmp$3.$implements$ReceivePortImpl$Dart)) {
    return this.visitReceivePort$member(x);
  }
  if (!!(tmp$4 = x , tmp$4 != null && tmp$4.$implements$ReceivePortSingleShotImpl$Dart)) {
    return this.visitReceivePortSingleShot$member(x);
  }
  $Dart$ThrowException('Message serialization: Illegal value ' + $toString(x) + ' passed');
}
;
MessageTraverser$Dart.prototype._dispatch$$named_ = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MessageTraverser$Dart.prototype._dispatch$$member_.call(this, x);
}
;
MessageTraverser$Dart.prototype._dispatch$$getter_ = function _dispatch$$getter_(){
  return $bind(MessageTraverser$Dart.prototype._dispatch$$named_, this);
}
;
MessageTraverser$Dart.prototype.visitPrimitive$member = function(x){
}
;
MessageTraverser$Dart.prototype.visitPrimitive$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MessageTraverser$Dart.prototype.visitPrimitive$member.call(this, x);
}
;
MessageTraverser$Dart.prototype.visitPrimitive$getter = function visitPrimitive$getter(){
  return $bind(MessageTraverser$Dart.prototype.visitPrimitive$named, this);
}
;
MessageTraverser$Dart.prototype.visitList$member = function(x){
}
;
MessageTraverser$Dart.prototype.visitList$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MessageTraverser$Dart.prototype.visitList$member.call(this, x);
}
;
MessageTraverser$Dart.prototype.visitList$getter = function visitList$getter(){
  return $bind(MessageTraverser$Dart.prototype.visitList$named, this);
}
;
MessageTraverser$Dart.prototype.visitMap$member = function(x){
}
;
MessageTraverser$Dart.prototype.visitMap$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MessageTraverser$Dart.prototype.visitMap$member.call(this, x);
}
;
MessageTraverser$Dart.prototype.visitMap$getter = function visitMap$getter(){
  return $bind(MessageTraverser$Dart.prototype.visitMap$named, this);
}
;
MessageTraverser$Dart.prototype.visitSendPort$member = function(x){
}
;
MessageTraverser$Dart.prototype.visitSendPort$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MessageTraverser$Dart.prototype.visitSendPort$member.call(this, x);
}
;
MessageTraverser$Dart.prototype.visitSendPort$getter = function visitSendPort$getter(){
  return $bind(MessageTraverser$Dart.prototype.visitSendPort$named, this);
}
;
MessageTraverser$Dart.prototype.visitReceivePort$member = function(x){
}
;
MessageTraverser$Dart.prototype.visitReceivePort$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MessageTraverser$Dart.prototype.visitReceivePort$member.call(this, x);
}
;
MessageTraverser$Dart.prototype.visitReceivePort$getter = function visitReceivePort$getter(){
  return $bind(MessageTraverser$Dart.prototype.visitReceivePort$named, this);
}
;
MessageTraverser$Dart.prototype.visitReceivePortSingleShot$member = function(x){
}
;
MessageTraverser$Dart.prototype.visitReceivePortSingleShot$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MessageTraverser$Dart.prototype.visitReceivePortSingleShot$member.call(this, x);
}
;
MessageTraverser$Dart.prototype.visitReceivePortSingleShot$getter = function visitReceivePortSingleShot$getter(){
  return $bind(MessageTraverser$Dart.prototype.visitReceivePortSingleShot$named, this);
}
;
MessageTraverser$Dart.prototype._taggedObjects$$named_ = function(){
  return this._taggedObjects$$getter_().apply(this, arguments);
}
;
MessageTraverser$Dart.prototype._taggedObjects$$getter_ = function(){
  return this._taggedObjects$$field_;
}
;
MessageTraverser$Dart.prototype._taggedObjects$$setter_ = function(tmp$0){
  this._taggedObjects$$field_ = tmp$0;
}
;
MessageTraverser$Dart.prototype._clearAttachedInfo$$member_ = function(obj){
  return native_MessageTraverser__clearAttachedInfo.call(this, obj);
}
;
MessageTraverser$Dart.prototype._clearAttachedInfo$$named_ = function($n, $o, obj){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MessageTraverser$Dart.prototype._clearAttachedInfo$$member_.call(this, obj);
}
;
MessageTraverser$Dart.prototype._clearAttachedInfo$$getter_ = function _clearAttachedInfo$$getter_(){
  return $bind(MessageTraverser$Dart.prototype._clearAttachedInfo$$named_, this);
}
;
MessageTraverser$Dart.prototype._setAttachedInfo$$member_ = function(o, info){
  return native_MessageTraverser__setAttachedInfo.call(this, o, info);
}
;
MessageTraverser$Dart.prototype._setAttachedInfo$$named_ = function($n, $o, o, info){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return MessageTraverser$Dart.prototype._setAttachedInfo$$member_.call(this, o, info);
}
;
MessageTraverser$Dart.prototype._setAttachedInfo$$getter_ = function _setAttachedInfo$$getter_(){
  return $bind(MessageTraverser$Dart.prototype._setAttachedInfo$$named_, this);
}
;
MessageTraverser$Dart.prototype._getAttachedInfo$$member_ = function(o){
  return native_MessageTraverser__getAttachedInfo.call(this, o);
}
;
MessageTraverser$Dart.prototype._getAttachedInfo$$named_ = function($n, $o, o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MessageTraverser$Dart.prototype._getAttachedInfo$$member_.call(this, o);
}
;
MessageTraverser$Dart.prototype._getAttachedInfo$$getter_ = function _getAttachedInfo$$getter_(){
  return $bind(MessageTraverser$Dart.prototype._getAttachedInfo$$named_, this);
}
;
function Copier$Dart(){
}

Copier$Dart.$lookupRTT = function(){
  return RTT.create($cls('Copier$Dart'), Copier$Dart.$RTTimplements);
}
;
Copier$Dart.$RTTimplements = function(rtt){
  Copier$Dart.$addTo(rtt);
}
;
Copier$Dart.$addTo = function(target){
  var rtt = Copier$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  MessageTraverser$Dart.$addTo(target);
}
;
Copier$Dart.prototype.$implements$Copier$Dart = 1;
Copier$Dart.prototype.$implements$MessageTraverser$Dart = 1;
Copier$Dart.prototype.$implements$Object$Dart = 1;
$inherits(Copier$Dart, MessageTraverser$Dart);
Copier$Dart.$Constructor = function(){
  MessageTraverser$Dart.$Constructor.call(this);
}
;
Copier$Dart.$Initializer = function(){
  MessageTraverser$Dart.$Initializer.call(this);
}
;
Copier$Dart.Copier$$Factory = function(){
  var tmp$0 = new Copier$Dart;
  tmp$0.$typeInfo = Copier$Dart.$lookupRTT();
  Copier$Dart.$Initializer.call(tmp$0);
  Copier$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
Copier$Dart.prototype.visitPrimitive$member = function(x){
  return x;
}
;
Copier$Dart.prototype.visitPrimitive$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Copier$Dart.prototype.visitPrimitive$member.call(this, x);
}
;
Copier$Dart.prototype.visitPrimitive$getter = function visitPrimitive$getter(){
  return $bind(Copier$Dart.prototype.visitPrimitive$named, this);
}
;
Copier$Dart.prototype.visitList$member = function(list){
  var tmp$1, tmp$0;
  var copy = this._getInfo$$member_(list);
  if (copy != null) {
    return copy;
  }
  var len = list.length$getter();
  copy = ListFactory$Dart.List$$Factory(null, len);
  this._attachInfo$$member_(list, copy);
  {
    var i = 0;
    for (; LT$operator(i, len); tmp$0 = i , (i = ADD$operator(tmp$0, 1) , tmp$0)) {
      copy.ASSIGN_INDEX$operator(i, tmp$1 = this._dispatch$$member_(list.INDEX$operator(i))) , tmp$1;
    }
  }
  return copy;
}
;
Copier$Dart.prototype.visitList$named = function($n, $o, list){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Copier$Dart.prototype.visitList$member.call(this, list);
}
;
Copier$Dart.prototype.visitList$getter = function visitList$getter(){
  return $bind(Copier$Dart.prototype.visitList$named, this);
}
;
function Copier$Dart$visitMap$c0$11_11$Hoisted(dartc_scp$1, key, val){
  var tmp$0;
  dartc_scp$1.copy.ASSIGN_INDEX$operator(this._dispatch$$member_(key), tmp$0 = this._dispatch$$member_(val)) , tmp$0;
}

function Copier$Dart$visitMap$c0$11_11$Hoisted$named($s0, $n, $o, key, val){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Copier$Dart$visitMap$c0$11_11$Hoisted.call(this, $s0, key, val);
}

Copier$Dart.prototype.visitMap$member = function(map){
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.copy = this._getInfo$$member_(map);
  if (dartc_scp$1.copy != null) {
    return dartc_scp$1.copy;
  }
  dartc_scp$1.copy = HashMapImplementation$Dart.HashMapImplementation$$Factory(HashMapImplementation$Dart.$lookupRTT());
  this._attachInfo$$member_(map, dartc_scp$1.copy);
  map.forEach$named(1, $noargs, $bind(Copier$Dart$visitMap$c0$11_11$Hoisted$named, this, dartc_scp$1));
  return dartc_scp$1.copy;
  dartc_scp$1 = $Dart$Null;
}
;
Copier$Dart.prototype.visitMap$named = function($n, $o, map){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Copier$Dart.prototype.visitMap$member.call(this, map);
}
;
Copier$Dart.prototype.visitMap$getter = function visitMap$getter(){
  return $bind(Copier$Dart.prototype.visitMap$named, this);
}
;
Copier$Dart.prototype.visitSendPort$member = function(port){
  return SendPortImpl$Dart.SendPortImpl$$Factory(port._workerId$$getter_(), port._isolateId$$getter_(), port._receivePortId$$getter_());
}
;
Copier$Dart.prototype.visitSendPort$named = function($n, $o, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Copier$Dart.prototype.visitSendPort$member.call(this, port);
}
;
Copier$Dart.prototype.visitSendPort$getter = function visitSendPort$getter(){
  return $bind(Copier$Dart.prototype.visitSendPort$named, this);
}
;
Copier$Dart.prototype.visitReceivePort$member = function(port){
  return port._toNewSendPort$$named_(0, $noargs);
}
;
Copier$Dart.prototype.visitReceivePort$named = function($n, $o, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Copier$Dart.prototype.visitReceivePort$member.call(this, port);
}
;
Copier$Dart.prototype.visitReceivePort$getter = function visitReceivePort$getter(){
  return $bind(Copier$Dart.prototype.visitReceivePort$named, this);
}
;
Copier$Dart.prototype.visitReceivePortSingleShot$member = function(port){
  return port._toNewSendPort$$named_(0, $noargs);
}
;
Copier$Dart.prototype.visitReceivePortSingleShot$named = function($n, $o, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Copier$Dart.prototype.visitReceivePortSingleShot$member.call(this, port);
}
;
Copier$Dart.prototype.visitReceivePortSingleShot$getter = function visitReceivePortSingleShot$getter(){
  return $bind(Copier$Dart.prototype.visitReceivePortSingleShot$named, this);
}
;
function Serializer$Dart(){
}

Serializer$Dart.$lookupRTT = function(){
  return RTT.create($cls('Serializer$Dart'), Serializer$Dart.$RTTimplements);
}
;
Serializer$Dart.$RTTimplements = function(rtt){
  Serializer$Dart.$addTo(rtt);
}
;
Serializer$Dart.$addTo = function(target){
  var rtt = Serializer$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  MessageTraverser$Dart.$addTo(target);
}
;
Serializer$Dart.prototype.$implements$Serializer$Dart = 1;
Serializer$Dart.prototype.$implements$MessageTraverser$Dart = 1;
Serializer$Dart.prototype.$implements$Object$Dart = 1;
$inherits(Serializer$Dart, MessageTraverser$Dart);
Serializer$Dart.$Constructor = function(){
  MessageTraverser$Dart.$Constructor.call(this);
}
;
Serializer$Dart.$Initializer = function(){
  MessageTraverser$Dart.$Initializer.call(this);
  this._nextFreeRefId$$field_ = 0;
}
;
Serializer$Dart.Serializer$$Factory = function(){
  var tmp$0 = new Serializer$Dart;
  tmp$0.$typeInfo = Serializer$Dart.$lookupRTT();
  Serializer$Dart.$Initializer.call(tmp$0);
  Serializer$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
Serializer$Dart.prototype.visitPrimitive$member = function(x){
  return x;
}
;
Serializer$Dart.prototype.visitPrimitive$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Serializer$Dart.prototype.visitPrimitive$member.call(this, x);
}
;
Serializer$Dart.prototype.visitPrimitive$getter = function visitPrimitive$getter(){
  return $bind(Serializer$Dart.prototype.visitPrimitive$named, this);
}
;
Serializer$Dart.prototype.visitList$member = function(list){
  var tmp$1, tmp$0;
  var copyId = this._getInfo$$member_(list);
  if (copyId != null) {
    return this._makeRef$$member_(copyId);
  }
  var id = (tmp$0 = this._nextFreeRefId$$getter_() , (this._nextFreeRefId$$setter_(tmp$1 = ADD$operator(tmp$0, 1)) , tmp$1 , tmp$0));
  this._attachInfo$$member_(list, id);
  var jsArray = this._serializeDartListIntoNewJsArray$$member_(list);
  return Serializer$Dart._dartListToJsArrayNoCopy$$member_(RTT.setTypeInfo(['list', id, jsArray], Array.$lookupRTT()));
}
;
Serializer$Dart.prototype.visitList$named = function($n, $o, list){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Serializer$Dart.prototype.visitList$member.call(this, list);
}
;
Serializer$Dart.prototype.visitList$getter = function visitList$getter(){
  return $bind(Serializer$Dart.prototype.visitList$named, this);
}
;
Serializer$Dart.prototype.visitMap$member = function(map){
  var tmp$1, tmp$0;
  var copyId = this._getInfo$$member_(map);
  if (copyId != null) {
    return this._makeRef$$member_(copyId);
  }
  var id = (tmp$0 = this._nextFreeRefId$$getter_() , (this._nextFreeRefId$$setter_(tmp$1 = ADD$operator(tmp$0, 1)) , tmp$1 , tmp$0));
  this._attachInfo$$member_(map, id);
  var keys = this._serializeDartListIntoNewJsArray$$member_(map.getKeys$named(0, $noargs));
  var values = this._serializeDartListIntoNewJsArray$$member_(map.getValues$named(0, $noargs));
  return Serializer$Dart._dartListToJsArrayNoCopy$$member_(RTT.setTypeInfo(['map', id, keys, values], Array.$lookupRTT()));
}
;
Serializer$Dart.prototype.visitMap$named = function($n, $o, map){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Serializer$Dart.prototype.visitMap$member.call(this, map);
}
;
Serializer$Dart.prototype.visitMap$getter = function visitMap$getter(){
  return $bind(Serializer$Dart.prototype.visitMap$named, this);
}
;
Serializer$Dart.prototype.visitSendPort$member = function(port){
  return Serializer$Dart._dartListToJsArrayNoCopy$$member_(RTT.setTypeInfo(['sendport', port._workerId$$getter_(), port._isolateId$$getter_(), port._receivePortId$$getter_()], Array.$lookupRTT()));
}
;
Serializer$Dart.prototype.visitSendPort$named = function($n, $o, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Serializer$Dart.prototype.visitSendPort$member.call(this, port);
}
;
Serializer$Dart.prototype.visitSendPort$getter = function visitSendPort$getter(){
  return $bind(Serializer$Dart.prototype.visitSendPort$named, this);
}
;
Serializer$Dart.prototype.visitReceivePort$member = function(port){
  return this.visitSendPort$member(port.toSendPort$named(0, $noargs));
  ;
}
;
Serializer$Dart.prototype.visitReceivePort$named = function($n, $o, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Serializer$Dart.prototype.visitReceivePort$member.call(this, port);
}
;
Serializer$Dart.prototype.visitReceivePort$getter = function visitReceivePort$getter(){
  return $bind(Serializer$Dart.prototype.visitReceivePort$named, this);
}
;
Serializer$Dart.prototype.visitReceivePortSingleShot$member = function(port){
  return this.visitSendPort$member(port.toSendPort$named(0, $noargs));
}
;
Serializer$Dart.prototype.visitReceivePortSingleShot$named = function($n, $o, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Serializer$Dart.prototype.visitReceivePortSingleShot$member.call(this, port);
}
;
Serializer$Dart.prototype.visitReceivePortSingleShot$getter = function visitReceivePortSingleShot$getter(){
  return $bind(Serializer$Dart.prototype.visitReceivePortSingleShot$named, this);
}
;
Serializer$Dart.prototype._serializeDartListIntoNewJsArray$$member_ = function(list){
  var tmp$0;
  var len = list.length$getter();
  var jsArray = Serializer$Dart._newJsArray$$member_(len);
  {
    var i = 0;
    for (; LT$operator(i, len); tmp$0 = i , (i = ADD$operator(tmp$0, 1) , tmp$0)) {
      Serializer$Dart._jsArrayIndexSet$$member_(jsArray, i, this._dispatch$$member_(list.INDEX$operator(i)));
    }
  }
  return jsArray;
}
;
Serializer$Dart.prototype._serializeDartListIntoNewJsArray$$named_ = function($n, $o, list){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Serializer$Dart.prototype._serializeDartListIntoNewJsArray$$member_.call(this, list);
}
;
Serializer$Dart.prototype._serializeDartListIntoNewJsArray$$getter_ = function _serializeDartListIntoNewJsArray$$getter_(){
  return $bind(Serializer$Dart.prototype._serializeDartListIntoNewJsArray$$named_, this);
}
;
Serializer$Dart.prototype._makeRef$$member_ = function(id){
  return Serializer$Dart._dartListToJsArrayNoCopy$$member_(RTT.setTypeInfo(['ref', id], Array.$lookupRTT()));
}
;
Serializer$Dart.prototype._makeRef$$named_ = function($n, $o, id){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Serializer$Dart.prototype._makeRef$$member_.call(this, id);
}
;
Serializer$Dart.prototype._makeRef$$getter_ = function _makeRef$$getter_(){
  return $bind(Serializer$Dart.prototype._makeRef$$named_, this);
}
;
Serializer$Dart.prototype._nextFreeRefId$$named_ = function(){
  return this._nextFreeRefId$$getter_().apply(this, arguments);
}
;
Serializer$Dart.prototype._nextFreeRefId$$getter_ = function(){
  return this._nextFreeRefId$$field_;
}
;
Serializer$Dart.prototype._nextFreeRefId$$setter_ = function(tmp$0){
  this._nextFreeRefId$$field_ = tmp$0;
}
;
Serializer$Dart._newJsArray$$member_ = function(len){
  return native_Serializer__newJsArray(len);
}
;
Serializer$Dart._newJsArray$$named_ = function($n, $o, len){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Serializer$Dart._newJsArray$$member_(len);
}
;
Serializer$Dart._newJsArray$$getter_ = function _newJsArray$$getter_(){
  return Serializer$Dart._newJsArray$$named_;
}
;
Serializer$Dart._jsArrayIndexSet$$member_ = function(jsArray, index, val){
  return native_Serializer__jsArrayIndexSet(jsArray, index, val);
}
;
Serializer$Dart._jsArrayIndexSet$$named_ = function($n, $o, jsArray, index, val){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return Serializer$Dart._jsArrayIndexSet$$member_(jsArray, index, val);
}
;
Serializer$Dart._jsArrayIndexSet$$getter_ = function _jsArrayIndexSet$$getter_(){
  return Serializer$Dart._jsArrayIndexSet$$named_;
}
;
Serializer$Dart._dartListToJsArrayNoCopy$$member_ = function(list){
  return native_Serializer__dartListToJsArrayNoCopy(list);
}
;
Serializer$Dart._dartListToJsArrayNoCopy$$named_ = function($n, $o, list){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Serializer$Dart._dartListToJsArrayNoCopy$$member_(list);
}
;
Serializer$Dart._dartListToJsArrayNoCopy$$getter_ = function _dartListToJsArrayNoCopy$$getter_(){
  return Serializer$Dart._dartListToJsArrayNoCopy$$named_;
}
;
function Deserializer$Dart(){
}

Deserializer$Dart.$lookupRTT = function(){
  return RTT.create($cls('Deserializer$Dart'));
}
;
Deserializer$Dart.$addTo = function(target){
  var rtt = Deserializer$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
Deserializer$Dart.prototype.$implements$Deserializer$Dart = 1;
Deserializer$Dart.prototype.$implements$Object$Dart = 1;
Deserializer$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
Deserializer$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
Deserializer$Dart.Deserializer$$Factory = function(){
  var tmp$0 = new Deserializer$Dart;
  tmp$0.$typeInfo = Deserializer$Dart.$lookupRTT();
  Deserializer$Dart.$Initializer.call(tmp$0);
  Deserializer$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
Deserializer$Dart.isPrimitive$member = function(x){
  var tmp$0;
  return x == null || String.$instanceOf(x) || !!(tmp$0 = x , tmp$0 != null && tmp$0.$implements$num$Dart) || Boolean.$instanceOf(x);
}
;
Deserializer$Dart.isPrimitive$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Deserializer$Dart.isPrimitive$member(x);
}
;
Deserializer$Dart.isPrimitive$getter = function isPrimitive$getter(){
  return Deserializer$Dart.isPrimitive$named;
}
;
Deserializer$Dart.prototype.deserialize$member = function(x){
  var tmp$0;
  if (Deserializer$Dart.isPrimitive$member(x)) {
    return x;
  }
  this._deserialized$$setter_(tmp$0 = HashMapImplementation$Dart.HashMapImplementation$$Factory(HashMapImplementation$Dart.$lookupRTT())) , tmp$0;
  return this._deserializeHelper$$member_(x);
}
;
Deserializer$Dart.prototype.deserialize$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Deserializer$Dart.prototype.deserialize$member.call(this, x);
}
;
Deserializer$Dart.prototype.deserialize$getter = function deserialize$getter(){
  return $bind(Deserializer$Dart.prototype.deserialize$named, this);
}
;
Deserializer$Dart.prototype._deserializeHelper$$member_ = function(x){
  if (Deserializer$Dart.isPrimitive$member(x)) {
    return x;
  }
  assert(Deserializer$Dart._isJsArray$$member_(x));
  switch (Deserializer$Dart._jsArrayIndex$$member_(x, 0)) {
    case 'ref':
      return this._deserializeRef$$member_(x);
    case 'list':
      return this._deserializeList$$member_(x);
    case 'map':
      return this._deserializeMap$$member_(x);
    case 'sendport':
      return this._deserializeSendPort$$member_(x);
    default:{
        $Dart$ThrowException('Unexpected serialized object');
      }

  }
}
;
Deserializer$Dart.prototype._deserializeHelper$$named_ = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Deserializer$Dart.prototype._deserializeHelper$$member_.call(this, x);
}
;
Deserializer$Dart.prototype._deserializeHelper$$getter_ = function _deserializeHelper$$getter_(){
  return $bind(Deserializer$Dart.prototype._deserializeHelper$$named_, this);
}
;
Deserializer$Dart.prototype._deserializeRef$$member_ = function(x){
  var id = Deserializer$Dart._jsArrayIndex$$member_(x, 1);
  var result = this._deserialized$$getter_().INDEX$operator(id);
  assert(result != null);
  return result;
}
;
Deserializer$Dart.prototype._deserializeRef$$named_ = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Deserializer$Dart.prototype._deserializeRef$$member_.call(this, x);
}
;
Deserializer$Dart.prototype._deserializeRef$$getter_ = function _deserializeRef$$getter_(){
  return $bind(Deserializer$Dart.prototype._deserializeRef$$named_, this);
}
;
Deserializer$Dart.prototype._deserializeList$$member_ = function(x){
  var tmp$1, tmp$2, tmp$0;
  var id = Deserializer$Dart._jsArrayIndex$$member_(x, 1);
  var jsArray = Deserializer$Dart._jsArrayIndex$$member_(x, 2);
  assert(Deserializer$Dart._isJsArray$$member_(jsArray));
  var dartList = this._jsArrayToDartListNoCopy$$member_(jsArray);
  this._deserialized$$getter_().ASSIGN_INDEX$operator(id, tmp$0 = dartList) , tmp$0;
  var len = dartList.length$getter();
  {
    var i = 0;
    for (; LT$operator(i, len); tmp$1 = i , (i = ADD$operator(tmp$1, 1) , tmp$1)) {
      dartList.ASSIGN_INDEX$operator(i, tmp$2 = this._deserializeHelper$$member_(dartList.INDEX$operator(i))) , tmp$2;
    }
  }
  return dartList;
}
;
Deserializer$Dart.prototype._deserializeList$$named_ = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Deserializer$Dart.prototype._deserializeList$$member_.call(this, x);
}
;
Deserializer$Dart.prototype._deserializeList$$getter_ = function _deserializeList$$getter_(){
  return $bind(Deserializer$Dart.prototype._deserializeList$$named_, this);
}
;
Deserializer$Dart.prototype._deserializeMap$$member_ = function(x){
  var tmp$1, tmp$2, tmp$0;
  var result = HashMapImplementation$Dart.HashMapImplementation$$Factory(HashMapImplementation$Dart.$lookupRTT());
  var id = Deserializer$Dart._jsArrayIndex$$member_(x, 1);
  this._deserialized$$getter_().ASSIGN_INDEX$operator(id, tmp$0 = result) , tmp$0;
  var keys = Deserializer$Dart._jsArrayIndex$$member_(x, 2);
  var values = Deserializer$Dart._jsArrayIndex$$member_(x, 3);
  assert(Deserializer$Dart._isJsArray$$member_(keys));
  assert(Deserializer$Dart._isJsArray$$member_(values));
  var len = Deserializer$Dart._jsArrayLength$$member_(keys);
  assert(EQ$operator(len, Deserializer$Dart._jsArrayLength$$member_(values)));
  {
    var i = 0;
    for (; LT$operator(i, len); tmp$1 = i , (i = ADD$operator(tmp$1, 1) , tmp$1)) {
      var key = this._deserializeHelper$$member_(Deserializer$Dart._jsArrayIndex$$member_(keys, i));
      var value = this._deserializeHelper$$member_(Deserializer$Dart._jsArrayIndex$$member_(values, i));
      result.ASSIGN_INDEX$operator(key, tmp$2 = value) , tmp$2;
    }
  }
  return result;
}
;
Deserializer$Dart.prototype._deserializeMap$$named_ = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Deserializer$Dart.prototype._deserializeMap$$member_.call(this, x);
}
;
Deserializer$Dart.prototype._deserializeMap$$getter_ = function _deserializeMap$$getter_(){
  return $bind(Deserializer$Dart.prototype._deserializeMap$$named_, this);
}
;
Deserializer$Dart.prototype._deserializeSendPort$$member_ = function(x){
  var workerId = Deserializer$Dart._jsArrayIndex$$member_(x, 1);
  var isolateId = Deserializer$Dart._jsArrayIndex$$member_(x, 2);
  var receivePortId = Deserializer$Dart._jsArrayIndex$$member_(x, 3);
  return SendPortImpl$Dart.SendPortImpl$$Factory(workerId, isolateId, receivePortId);
}
;
Deserializer$Dart.prototype._deserializeSendPort$$named_ = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Deserializer$Dart.prototype._deserializeSendPort$$member_.call(this, x);
}
;
Deserializer$Dart.prototype._deserializeSendPort$$getter_ = function _deserializeSendPort$$getter_(){
  return $bind(Deserializer$Dart.prototype._deserializeSendPort$$named_, this);
}
;
Deserializer$Dart.prototype._jsArrayToDartListNoCopy$$member_ = function(a){
  var tmp$0;
  assert(!!(tmp$0 = a , tmp$0 != null && tmp$0.$implements$List$Dart));
  return a;
}
;
Deserializer$Dart.prototype._jsArrayToDartListNoCopy$$named_ = function($n, $o, a){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Deserializer$Dart.prototype._jsArrayToDartListNoCopy$$member_.call(this, a);
}
;
Deserializer$Dart.prototype._jsArrayToDartListNoCopy$$getter_ = function _jsArrayToDartListNoCopy$$getter_(){
  return $bind(Deserializer$Dart.prototype._jsArrayToDartListNoCopy$$named_, this);
}
;
Deserializer$Dart.prototype._deserialized$$named_ = function(){
  return this._deserialized$$getter_().apply(this, arguments);
}
;
Deserializer$Dart.prototype._deserialized$$getter_ = function(){
  return this._deserialized$$field_;
}
;
Deserializer$Dart.prototype._deserialized$$setter_ = function(tmp$0){
  this._deserialized$$field_ = tmp$0;
}
;
Deserializer$Dart._isJsArray$$member_ = function(x){
  return native_Deserializer__isJsArray(x);
}
;
Deserializer$Dart._isJsArray$$named_ = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Deserializer$Dart._isJsArray$$member_(x);
}
;
Deserializer$Dart._isJsArray$$getter_ = function _isJsArray$$getter_(){
  return Deserializer$Dart._isJsArray$$named_;
}
;
Deserializer$Dart._jsArrayIndex$$member_ = function(x, index){
  return native_Deserializer__jsArrayIndex(x, index);
}
;
Deserializer$Dart._jsArrayIndex$$named_ = function($n, $o, x, index){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Deserializer$Dart._jsArrayIndex$$member_(x, index);
}
;
Deserializer$Dart._jsArrayIndex$$getter_ = function _jsArrayIndex$$getter_(){
  return Deserializer$Dart._jsArrayIndex$$named_;
}
;
Deserializer$Dart._jsArrayLength$$member_ = function(x){
  return native_Deserializer__jsArrayLength(x);
}
;
Deserializer$Dart._jsArrayLength$$named_ = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Deserializer$Dart._jsArrayLength$$member_(x);
}
;
Deserializer$Dart._jsArrayLength$$getter_ = function _jsArrayLength$$getter_(){
  return Deserializer$Dart._jsArrayLength$$named_;
}
;
function MathNatives$Dart(){
}

MathNatives$Dart.$lookupRTT = function(){
  return RTT.create($cls('MathNatives$Dart'));
}
;
MathNatives$Dart.$addTo = function(target){
  var rtt = MathNatives$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
MathNatives$Dart.prototype.$implements$MathNatives$Dart = 1;
MathNatives$Dart.prototype.$implements$Object$Dart = 1;
MathNatives$Dart.cos$member = function(d){
  return native_MathNatives_cos(d);
}
;
MathNatives$Dart.cos$named = function($n, $o, d){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MathNatives$Dart.cos$member(d);
}
;
MathNatives$Dart.cos$getter = function cos$getter(){
  return MathNatives$Dart.cos$named;
}
;
MathNatives$Dart.sin$member = function(d){
  return native_MathNatives_sin(d);
}
;
MathNatives$Dart.sin$named = function($n, $o, d){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MathNatives$Dart.sin$member(d);
}
;
MathNatives$Dart.sin$getter = function sin$getter(){
  return MathNatives$Dart.sin$named;
}
;
MathNatives$Dart.tan$member = function(d){
  return native_MathNatives_tan(d);
}
;
MathNatives$Dart.tan$named = function($n, $o, d){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MathNatives$Dart.tan$member(d);
}
;
MathNatives$Dart.tan$getter = function tan$getter(){
  return MathNatives$Dart.tan$named;
}
;
MathNatives$Dart.acos$member = function(d){
  return native_MathNatives_acos(d);
}
;
MathNatives$Dart.acos$named = function($n, $o, d){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MathNatives$Dart.acos$member(d);
}
;
MathNatives$Dart.acos$getter = function acos$getter(){
  return MathNatives$Dart.acos$named;
}
;
MathNatives$Dart.asin$member = function(d){
  return native_MathNatives_asin(d);
}
;
MathNatives$Dart.asin$named = function($n, $o, d){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MathNatives$Dart.asin$member(d);
}
;
MathNatives$Dart.asin$getter = function asin$getter(){
  return MathNatives$Dart.asin$named;
}
;
MathNatives$Dart.atan$member = function(d){
  return native_MathNatives_atan(d);
}
;
MathNatives$Dart.atan$named = function($n, $o, d){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MathNatives$Dart.atan$member(d);
}
;
MathNatives$Dart.atan$getter = function atan$getter(){
  return MathNatives$Dart.atan$named;
}
;
MathNatives$Dart.atan2$member = function(a, b){
  return native_MathNatives_atan2(a, b);
}
;
MathNatives$Dart.atan2$named = function($n, $o, a, b){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return MathNatives$Dart.atan2$member(a, b);
}
;
MathNatives$Dart.atan2$getter = function atan2$getter(){
  return MathNatives$Dart.atan2$named;
}
;
MathNatives$Dart.sqrt$member = function(d){
  return native_MathNatives_sqrt(d);
}
;
MathNatives$Dart.sqrt$named = function($n, $o, d){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MathNatives$Dart.sqrt$member(d);
}
;
MathNatives$Dart.sqrt$getter = function sqrt$getter(){
  return MathNatives$Dart.sqrt$named;
}
;
MathNatives$Dart.exp$member = function(d){
  return native_MathNatives_exp(d);
}
;
MathNatives$Dart.exp$named = function($n, $o, d){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MathNatives$Dart.exp$member(d);
}
;
MathNatives$Dart.exp$getter = function exp$getter(){
  return MathNatives$Dart.exp$named;
}
;
MathNatives$Dart.log$member = function(d){
  return native_MathNatives_log(d);
}
;
MathNatives$Dart.log$named = function($n, $o, d){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MathNatives$Dart.log$member(d);
}
;
MathNatives$Dart.log$getter = function log$getter(){
  return MathNatives$Dart.log$named;
}
;
MathNatives$Dart.pow$member = function(d1, d2){
  return native_MathNatives_pow(d1, d2);
}
;
MathNatives$Dart.pow$named = function($n, $o, d1, d2){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return MathNatives$Dart.pow$member(d1, d2);
}
;
MathNatives$Dart.pow$getter = function pow$getter(){
  return MathNatives$Dart.pow$named;
}
;
MathNatives$Dart.random$member = function(){
  return native_MathNatives_random();
}
;
MathNatives$Dart.random$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return MathNatives$Dart.random$member();
}
;
MathNatives$Dart.random$getter = function random$getter(){
  return MathNatives$Dart.random$named;
}
;
MathNatives$Dart.parseInt$member = function(str){
  return native_MathNatives_parseInt(str);
}
;
MathNatives$Dart.parseInt$named = function($n, $o, str){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MathNatives$Dart.parseInt$member(str);
}
;
MathNatives$Dart.parseInt$getter = function parseInt$getter(){
  return MathNatives$Dart.parseInt$named;
}
;
MathNatives$Dart.parseDouble$member = function(str){
  return native_MathNatives_parseDouble(str);
}
;
MathNatives$Dart.parseDouble$named = function($n, $o, str){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MathNatives$Dart.parseDouble$member(str);
}
;
MathNatives$Dart.parseDouble$getter = function parseDouble$getter(){
  return MathNatives$Dart.parseDouble$named;
}
;
MathNatives$Dart._newBadNumberFormat$$member_ = function(x){
  return BadNumberFormatException$Dart.BadNumberFormatException$$Factory(x);
}
;
MathNatives$Dart._newBadNumberFormat$$named_ = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return MathNatives$Dart._newBadNumberFormat$$member_(x);
}
;
function native_MathNatives__newBadNumberFormat(x){
  return MathNatives$Dart._newBadNumberFormat$$member_(x);
}

MathNatives$Dart._newBadNumberFormat$$getter_ = function _newBadNumberFormat$$getter_(){
  return MathNatives$Dart._newBadNumberFormat$$named_;
}
;
MathNatives$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
MathNatives$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
MathNatives$Dart.MathNatives$$Factory = function(){
  var tmp$0 = new MathNatives$Dart;
  tmp$0.$typeInfo = MathNatives$Dart.$lookupRTT();
  MathNatives$Dart.$Initializer.call(tmp$0);
  MathNatives$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
Number.$lookupRTT = function(){
  return RTT.create($cls('Number'), Number.$RTTimplements);
}
;
Number.$RTTimplements = function(rtt){
  Number.$addTo(rtt);
}
;
Number.$addTo = function(target){
  var rtt = Number.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  int$Dart.$addTo(target);
  double$Dart.$addTo(target);
}
;
Number.prototype.$implements$NumberImplementation$Dart = 1;
Number.prototype.$implements$int$Dart = 1;
Number.prototype.$implements$num$Dart = 1;
Number.prototype.$implements$Comparable$Dart = 1;
Number.prototype.$implements$Hashable$Dart = 1;
Number.prototype.$implements$double$Dart = 1;
Number.prototype.$implements$Object$Dart = 1;
Number.prototype.ADD$operator = function(other){
  return native_NumberImplementation_ADD.call(this, other);
}
;
Number.prototype.SUB$operator = function(other){
  return native_NumberImplementation_SUB.call(this, other);
}
;
Number.prototype.MUL$operator = function(other){
  return native_NumberImplementation_MUL.call(this, other);
}
;
Number.prototype.DIV$operator = function(other){
  return native_NumberImplementation_DIV.call(this, other);
}
;
Number.prototype.TRUNC$operator = function(other){
  return native_NumberImplementation_TRUNC.call(this, other);
}
;
Number.prototype.MOD$operator = function(shiftAmount){
  return native_NumberImplementation_MOD.call(this, shiftAmount);
}
;
Number.prototype.negate$operator = function(){
  return native_NumberImplementation_negate.call(this);
}
;
Number.prototype.BIT_OR$operator = function(other){
  return native_NumberImplementation_BIT_OR.call(this, other);
}
;
Number.prototype.BIT_AND$operator = function(other){
  return native_NumberImplementation_BIT_AND.call(this, other);
}
;
Number.prototype.BIT_XOR$operator = function(other){
  return native_NumberImplementation_BIT_XOR.call(this, other);
}
;
Number.prototype.SHL$operator = function(shiftAmount){
  return native_NumberImplementation_SHL.call(this, shiftAmount);
}
;
Number.prototype.SAR$operator = function(shiftAmount){
  return native_NumberImplementation_SAR.call(this, shiftAmount);
}
;
Number.prototype.BIT_NOT$operator = function(){
  return native_NumberImplementation_BIT_NOT.call(this);
}
;
Number.prototype.EQ$operator = function(other){
  return native_NumberImplementation_EQ.call(this, other);
}
;
Number.prototype.LT$operator = function(other){
  return native_NumberImplementation_LT.call(this, other);
}
;
Number.prototype.LTE$operator = function(other){
  return native_NumberImplementation_LTE.call(this, other);
}
;
Number.prototype.GT$operator = function(other){
  return native_NumberImplementation_GT.call(this, other);
}
;
Number.prototype.GTE$operator = function(other){
  return native_NumberImplementation_GTE.call(this, other);
}
;
Number.prototype.remainder$member = function(other){
  return native_NumberImplementation_remainder.call(this, other);
}
;
Number.prototype.remainder$named = function($n, $o, other){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Number.prototype.remainder$member.call(this, other);
}
;
Number.prototype.remainder$getter = function remainder$getter(){
  return $bind(Number.prototype.remainder$named, this);
}
;
Number.prototype.abs$member = function(){
  return native_NumberImplementation_abs.call(this);
}
;
Number.prototype.abs$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Number.prototype.abs$member.call(this);
}
;
Number.prototype.abs$getter = function abs$getter(){
  return $bind(Number.prototype.abs$named, this);
}
;
Number.prototype.round$member = function(){
  return native_NumberImplementation_round.call(this);
}
;
Number.prototype.round$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Number.prototype.round$member.call(this);
}
;
Number.prototype.round$getter = function round$getter(){
  return $bind(Number.prototype.round$named, this);
}
;
Number.prototype.floor$member = function(){
  return native_NumberImplementation_floor.call(this);
}
;
Number.prototype.floor$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Number.prototype.floor$member.call(this);
}
;
Number.prototype.floor$getter = function floor$getter(){
  return $bind(Number.prototype.floor$named, this);
}
;
Number.prototype.ceil$member = function(){
  return native_NumberImplementation_ceil.call(this);
}
;
Number.prototype.ceil$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Number.prototype.ceil$member.call(this);
}
;
Number.prototype.ceil$getter = function ceil$getter(){
  return $bind(Number.prototype.ceil$named, this);
}
;
Number.prototype.truncate$member = function(){
  return native_NumberImplementation_truncate.call(this);
}
;
Number.prototype.truncate$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Number.prototype.truncate$member.call(this);
}
;
Number.prototype.truncate$getter = function truncate$getter(){
  return $bind(Number.prototype.truncate$named, this);
}
;
Number.prototype.compareTo$member = function(other){
  return SUB$operator(this, other);
}
;
Number.prototype.compareTo$named = function($n, $o, other){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Number.prototype.compareTo$member.call(this, other);
}
;
Number.prototype.compareTo$getter = function compareTo$getter(){
  return $bind(Number.prototype.compareTo$named, this);
}
;
Number.prototype.isNegative$member = function(){
  return native_NumberImplementation_isNegative.call(this);
}
;
Number.prototype.isNegative$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Number.prototype.isNegative$member.call(this);
}
;
Number.prototype.isNegative$getter = function isNegative$getter(){
  return $bind(Number.prototype.isNegative$named, this);
}
;
Number.prototype.isEven$member = function(){
  return native_NumberImplementation_isEven.call(this);
}
;
Number.prototype.isEven$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Number.prototype.isEven$member.call(this);
}
;
Number.prototype.isEven$getter = function isEven$getter(){
  return $bind(Number.prototype.isEven$named, this);
}
;
Number.prototype.isOdd$member = function(){
  return native_NumberImplementation_isOdd.call(this);
}
;
Number.prototype.isOdd$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Number.prototype.isOdd$member.call(this);
}
;
Number.prototype.isOdd$getter = function isOdd$getter(){
  return $bind(Number.prototype.isOdd$named, this);
}
;
Number.prototype.isNaN$member = function(){
  return native_NumberImplementation_isNaN.call(this);
}
;
Number.prototype.isNaN$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Number.prototype.isNaN$member.call(this);
}
;
Number.prototype.isNaN$getter = function isNaN$getter(){
  return $bind(Number.prototype.isNaN$named, this);
}
;
Number.prototype.isInfinite$member = function(){
  return native_NumberImplementation_isInfinite.call(this);
}
;
Number.prototype.isInfinite$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Number.prototype.isInfinite$member.call(this);
}
;
Number.prototype.isInfinite$getter = function isInfinite$getter(){
  return $bind(Number.prototype.isInfinite$named, this);
}
;
Number.prototype.toInt$member = function(){
  if (this.isNaN$member()) {
    $Dart$ThrowException(BadNumberFormatException$Dart.BadNumberFormatException$$Factory('NaN'));
  }
  if (this.isInfinite$member()) {
    $Dart$ThrowException(BadNumberFormatException$Dart.BadNumberFormatException$$Factory('Infinity'));
  }
  var truncated = this.truncate$member();
  if (EQ$operator(truncated, negate$operator(0))) {
    return 0;
  }
  return truncated;
}
;
Number.prototype.toInt$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Number.prototype.toInt$member.call(this);
}
;
Number.prototype.toInt$getter = function toInt$getter(){
  return $bind(Number.prototype.toInt$named, this);
}
;
Number.prototype.toDouble$member = function(){
  return this;
}
;
Number.prototype.toDouble$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Number.prototype.toDouble$member.call(this);
}
;
Number.prototype.toDouble$getter = function toDouble$getter(){
  return $bind(Number.prototype.toDouble$named, this);
}
;
Number.prototype.toString$member = function(){
  return native_NumberImplementation_toString.call(this);
}
;
Number.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Number.prototype.toString$member.call(this);
}
;
Number.prototype.toString$getter = function toString$getter(){
  return $bind(Number.prototype.toString$named, this);
}
;
Number.prototype.toStringAsFixed$member = function(fractionDigits){
  return native_NumberImplementation_toStringAsFixed.call(this, fractionDigits);
}
;
Number.prototype.toStringAsFixed$named = function($n, $o, fractionDigits){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Number.prototype.toStringAsFixed$member.call(this, fractionDigits);
}
;
Number.prototype.toStringAsFixed$getter = function toStringAsFixed$getter(){
  return $bind(Number.prototype.toStringAsFixed$named, this);
}
;
Number.prototype.toStringAsExponential$member = function(fractionDigits){
  return native_NumberImplementation_toStringAsExponential.call(this, fractionDigits);
}
;
Number.prototype.toStringAsExponential$named = function($n, $o, fractionDigits){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Number.prototype.toStringAsExponential$member.call(this, fractionDigits);
}
;
Number.prototype.toStringAsExponential$getter = function toStringAsExponential$getter(){
  return $bind(Number.prototype.toStringAsExponential$named, this);
}
;
Number.prototype.toStringAsPrecision$member = function(precision){
  return native_NumberImplementation_toStringAsPrecision.call(this, precision);
}
;
Number.prototype.toStringAsPrecision$named = function($n, $o, precision){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Number.prototype.toStringAsPrecision$member.call(this, precision);
}
;
Number.prototype.toStringAsPrecision$getter = function toStringAsPrecision$getter(){
  return $bind(Number.prototype.toStringAsPrecision$named, this);
}
;
Number.prototype.toRadixString$member = function(radix){
  return native_NumberImplementation_toRadixString.call(this, radix);
}
;
Number.prototype.toRadixString$named = function($n, $o, radix){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Number.prototype.toRadixString$member.call(this, radix);
}
;
Number.prototype.toRadixString$getter = function toRadixString$getter(){
  return $bind(Number.prototype.toRadixString$named, this);
}
;
Number.prototype.hashCode$member = function(){
  return native_NumberImplementation_hashCode.call(this);
}
;
Number.prototype.hashCode$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Number.prototype.hashCode$member.call(this);
}
;
Number.prototype.hashCode$getter = function hashCode$getter(){
  return $bind(Number.prototype.hashCode$named, this);
}
;
Number.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
Number.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
Number.NumberImplementation$$Factory = function(){
  var tmp$0 = new Number;
  tmp$0.$typeInfo = Number.$lookupRTT();
  Number.$Initializer.call(tmp$0);
  Number.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function JSSyntaxRegExp$Dart(){
}

JSSyntaxRegExp$Dart.$lookupRTT = function(){
  return RTT.create($cls('JSSyntaxRegExp$Dart'), JSSyntaxRegExp$Dart.$RTTimplements);
}
;
JSSyntaxRegExp$Dart.$RTTimplements = function(rtt){
  JSSyntaxRegExp$Dart.$addTo(rtt);
}
;
JSSyntaxRegExp$Dart.$addTo = function(target){
  var rtt = JSSyntaxRegExp$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  RegExp$Dart.$addTo(target);
}
;
JSSyntaxRegExp$Dart.prototype.$implements$JSSyntaxRegExp$Dart = 1;
JSSyntaxRegExp$Dart.prototype.$implements$RegExp$Dart = 1;
JSSyntaxRegExp$Dart.prototype.$implements$Pattern$Dart = 1;
JSSyntaxRegExp$Dart.prototype.$implements$Object$Dart = 1;
JSSyntaxRegExp$Dart.$Constructor = function(pattern, multiLine, ignoreCase){
  Object.$Constructor.call(this);
}
;
JSSyntaxRegExp$Dart.$Initializer = function(pattern, multiLine, ignoreCase){
  Object.$Initializer.call(this);
  this.pattern$field = pattern;
  this.multiLine$field = multiLine;
  this.ignoreCase$field = ignoreCase;
}
;
JSSyntaxRegExp$Dart.JSSyntaxRegExp$$Factory = function(pattern, multiLine, ignoreCase){
  var tmp$0 = new JSSyntaxRegExp$Dart;
  tmp$0.$typeInfo = JSSyntaxRegExp$Dart.$lookupRTT();
  JSSyntaxRegExp$Dart.$Initializer.call(tmp$0, pattern, multiLine, ignoreCase);
  JSSyntaxRegExp$Dart.$Constructor.call(tmp$0, pattern, multiLine, ignoreCase);
  return tmp$0;
}
;
JSSyntaxRegExp$Dart.prototype.pattern$named = function(){
  return this.pattern$getter().apply(this, arguments);
}
;
JSSyntaxRegExp$Dart.prototype.pattern$getter = function(){
  return this.pattern$field;
}
;
JSSyntaxRegExp$Dart.prototype.multiLine$named = function(){
  return this.multiLine$getter().apply(this, arguments);
}
;
JSSyntaxRegExp$Dart.prototype.multiLine$getter = function(){
  return this.multiLine$field;
}
;
JSSyntaxRegExp$Dart.prototype.ignoreCase$named = function(){
  return this.ignoreCase$getter().apply(this, arguments);
}
;
JSSyntaxRegExp$Dart.prototype.ignoreCase$getter = function(){
  return this.ignoreCase$field;
}
;
JSSyntaxRegExp$Dart.prototype.allMatches$member = function(str){
  return _LazyAllMatches$Dart._LazyAllMatches$$Factory(this, str);
}
;
JSSyntaxRegExp$Dart.prototype.allMatches$named = function($n, $o, str){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return JSSyntaxRegExp$Dart.prototype.allMatches$member.call(this, str);
}
;
JSSyntaxRegExp$Dart.prototype.allMatches$getter = function allMatches$getter(){
  return $bind(JSSyntaxRegExp$Dart.prototype.allMatches$named, this);
}
;
JSSyntaxRegExp$Dart.prototype.firstMatch$member = function(str){
  return native_JSSyntaxRegExp_firstMatch.call(this, str);
}
;
JSSyntaxRegExp$Dart.prototype.firstMatch$named = function($n, $o, str){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return JSSyntaxRegExp$Dart.prototype.firstMatch$member.call(this, str);
}
;
JSSyntaxRegExp$Dart.prototype.firstMatch$getter = function firstMatch$getter(){
  return $bind(JSSyntaxRegExp$Dart.prototype.firstMatch$named, this);
}
;
JSSyntaxRegExp$Dart.prototype.hasMatch$member = function(str){
  return native_JSSyntaxRegExp_hasMatch.call(this, str);
}
;
JSSyntaxRegExp$Dart.prototype.hasMatch$named = function($n, $o, str){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return JSSyntaxRegExp$Dart.prototype.hasMatch$member.call(this, str);
}
;
JSSyntaxRegExp$Dart.prototype.hasMatch$getter = function hasMatch$getter(){
  return $bind(JSSyntaxRegExp$Dart.prototype.hasMatch$named, this);
}
;
JSSyntaxRegExp$Dart.prototype.stringMatch$member = function(str){
  return native_JSSyntaxRegExp_stringMatch.call(this, str);
}
;
JSSyntaxRegExp$Dart.prototype.stringMatch$named = function($n, $o, str){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return JSSyntaxRegExp$Dart.prototype.stringMatch$member.call(this, str);
}
;
JSSyntaxRegExp$Dart.prototype.stringMatch$getter = function stringMatch$getter(){
  return $bind(JSSyntaxRegExp$Dart.prototype.stringMatch$named, this);
}
;
JSSyntaxRegExp$Dart._pattern$$member_ = function(regexp){
  return regexp.pattern$getter();
}
;
JSSyntaxRegExp$Dart._pattern$$named_ = function($n, $o, regexp){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return JSSyntaxRegExp$Dart._pattern$$member_(regexp);
}
;
function native_JSSyntaxRegExp__pattern(regexp){
  return JSSyntaxRegExp$Dart._pattern$$member_(regexp);
}

JSSyntaxRegExp$Dart._pattern$$getter_ = function _pattern$$getter_(){
  return JSSyntaxRegExp$Dart._pattern$$named_;
}
;
JSSyntaxRegExp$Dart._multiLine$$member_ = function(regexp){
  return regexp.multiLine$getter();
}
;
JSSyntaxRegExp$Dart._multiLine$$named_ = function($n, $o, regexp){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return JSSyntaxRegExp$Dart._multiLine$$member_(regexp);
}
;
function native_JSSyntaxRegExp__multiLine(regexp){
  return JSSyntaxRegExp$Dart._multiLine$$member_(regexp);
}

JSSyntaxRegExp$Dart._multiLine$$getter_ = function _multiLine$$getter_(){
  return JSSyntaxRegExp$Dart._multiLine$$named_;
}
;
JSSyntaxRegExp$Dart._ignoreCase$$member_ = function(regexp){
  return regexp.ignoreCase$getter();
}
;
JSSyntaxRegExp$Dart._ignoreCase$$named_ = function($n, $o, regexp){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return JSSyntaxRegExp$Dart._ignoreCase$$member_(regexp);
}
;
function native_JSSyntaxRegExp__ignoreCase(regexp){
  return JSSyntaxRegExp$Dart._ignoreCase$$member_(regexp);
}

JSSyntaxRegExp$Dart._ignoreCase$$getter_ = function _ignoreCase$$getter_(){
  return JSSyntaxRegExp$Dart._ignoreCase$$named_;
}
;
JSSyntaxRegExp$Dart.prototype.$const_id = function(){
  return $cls('JSSyntaxRegExp$Dart') + (':' + $dart_const_id(this.pattern$field)) + (':' + $dart_const_id(this.multiLine$field)) + (':' + $dart_const_id(this.ignoreCase$field));
}
;
function JSSyntaxMatch$Dart(){
}

JSSyntaxMatch$Dart.$lookupRTT = function(){
  return RTT.create($cls('JSSyntaxMatch$Dart'), JSSyntaxMatch$Dart.$RTTimplements);
}
;
JSSyntaxMatch$Dart.$RTTimplements = function(rtt){
  JSSyntaxMatch$Dart.$addTo(rtt);
}
;
JSSyntaxMatch$Dart.$addTo = function(target){
  var rtt = JSSyntaxMatch$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Match$Dart.$addTo(target);
}
;
JSSyntaxMatch$Dart.prototype.$implements$JSSyntaxMatch$Dart = 1;
JSSyntaxMatch$Dart.prototype.$implements$Match$Dart = 1;
JSSyntaxMatch$Dart.prototype.$implements$Object$Dart = 1;
JSSyntaxMatch$Dart.$Constructor = function(regexp, str){
  Object.$Constructor.call(this);
}
;
JSSyntaxMatch$Dart.$Initializer = function(regexp, str){
  Object.$Initializer.call(this);
  this.pattern$field = regexp;
  this.str$field = str;
}
;
JSSyntaxMatch$Dart.JSSyntaxMatch$$Factory = function(regexp, str){
  var tmp$0 = new JSSyntaxMatch$Dart;
  tmp$0.$typeInfo = JSSyntaxMatch$Dart.$lookupRTT();
  JSSyntaxMatch$Dart.$Initializer.call(tmp$0, regexp, str);
  JSSyntaxMatch$Dart.$Constructor.call(tmp$0, regexp, str);
  return tmp$0;
}
;
JSSyntaxMatch$Dart.prototype.str$named = function(){
  return this.str$getter().apply(this, arguments);
}
;
JSSyntaxMatch$Dart.prototype.str$getter = function(){
  return this.str$field;
}
;
JSSyntaxMatch$Dart.prototype.pattern$named = function(){
  return this.pattern$getter().apply(this, arguments);
}
;
JSSyntaxMatch$Dart.prototype.pattern$getter = function(){
  return this.pattern$field;
}
;
JSSyntaxMatch$Dart.prototype.INDEX$operator = function(group){
  return this.group$named(1, $noargs, group);
}
;
function JSSyntaxMatch$Dart$groups$c0$18_18$Hoisted(dartc_scp$1, group){
  dartc_scp$1.strings.add$named(1, $noargs, this.group$named(1, $noargs, group));
}

function JSSyntaxMatch$Dart$groups$c0$18_18$Hoisted$named($s0, $n, $o, group){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return JSSyntaxMatch$Dart$groups$c0$18_18$Hoisted.call(this, $s0, group);
}

JSSyntaxMatch$Dart.prototype.groups$member = function(groups){
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.strings = ArrayFactory$Dart.Array$$Factory([String$Dart.$lookupRTT()], $Dart$Null);
  groups.forEach$named(1, $noargs, $bind(JSSyntaxMatch$Dart$groups$c0$18_18$Hoisted$named, this, dartc_scp$1));
  return dartc_scp$1.strings;
  dartc_scp$1 = $Dart$Null;
}
;
JSSyntaxMatch$Dart.prototype.groups$named = function($n, $o, groups){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return JSSyntaxMatch$Dart.prototype.groups$member.call(this, groups);
}
;
JSSyntaxMatch$Dart.prototype.groups$getter = function groups$getter(){
  return $bind(JSSyntaxMatch$Dart.prototype.groups$named, this);
}
;
JSSyntaxMatch$Dart.prototype.group$member = function(nb){
  return native_JSSyntaxMatch_group.call(this, nb);
}
;
JSSyntaxMatch$Dart.prototype.group$named = function($n, $o, nb){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return JSSyntaxMatch$Dart.prototype.group$member.call(this, nb);
}
;
JSSyntaxMatch$Dart.prototype.group$getter = function group$getter(){
  return $bind(JSSyntaxMatch$Dart.prototype.group$named, this);
}
;
JSSyntaxMatch$Dart.prototype.start$member = function(){
  return native_JSSyntaxMatch_start.call(this);
}
;
JSSyntaxMatch$Dart.prototype.start$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return JSSyntaxMatch$Dart.prototype.start$member.call(this);
}
;
JSSyntaxMatch$Dart.prototype.start$getter = function start$getter(){
  return $bind(JSSyntaxMatch$Dart.prototype.start$named, this);
}
;
JSSyntaxMatch$Dart.prototype.end$member = function(){
  return native_JSSyntaxMatch_end.call(this);
}
;
JSSyntaxMatch$Dart.prototype.end$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return JSSyntaxMatch$Dart.prototype.end$member.call(this);
}
;
JSSyntaxMatch$Dart.prototype.end$getter = function end$getter(){
  return $bind(JSSyntaxMatch$Dart.prototype.end$named, this);
}
;
JSSyntaxMatch$Dart.prototype.groupCount$member = function(){
  return native_JSSyntaxMatch_groupCount.call(this);
}
;
JSSyntaxMatch$Dart.prototype.groupCount$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return JSSyntaxMatch$Dart.prototype.groupCount$member.call(this);
}
;
JSSyntaxMatch$Dart.prototype.groupCount$getter = function groupCount$getter(){
  return $bind(JSSyntaxMatch$Dart.prototype.groupCount$named, this);
}
;
JSSyntaxMatch$Dart._new$$member_ = function(regexp, str){
  return JSSyntaxMatch$Dart.JSSyntaxMatch$$Factory(regexp, str);
}
;
JSSyntaxMatch$Dart._new$$named_ = function($n, $o, regexp, str){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return JSSyntaxMatch$Dart._new$$member_(regexp, str);
}
;
function native_JSSyntaxMatch__new(regexp, str){
  return JSSyntaxMatch$Dart._new$$member_(regexp, str);
}

JSSyntaxMatch$Dart._new$$getter_ = function _new$$getter_(){
  return JSSyntaxMatch$Dart._new$$named_;
}
;
JSSyntaxMatch$Dart.prototype.$const_id = function(){
  return $cls('JSSyntaxMatch$Dart') + (':' + $dart_const_id(this.str$field)) + (':' + $dart_const_id(this.pattern$field));
}
;
function _LazyAllMatches$Dart(){
}

_LazyAllMatches$Dart.$lookupRTT = function(){
  return RTT.create($cls('_LazyAllMatches$Dart'), _LazyAllMatches$Dart.$RTTimplements);
}
;
_LazyAllMatches$Dart.$RTTimplements = function(rtt){
  _LazyAllMatches$Dart.$addTo(rtt);
}
;
_LazyAllMatches$Dart.$addTo = function(target){
  var rtt = _LazyAllMatches$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Collection$Dart.$addTo(target, [Match$Dart.$lookupRTT()]);
}
;
_LazyAllMatches$Dart.prototype.$implements$_LazyAllMatches$Dart = 1;
_LazyAllMatches$Dart.prototype.$implements$Collection$Dart = 1;
_LazyAllMatches$Dart.prototype.$implements$Iterable$Dart = 1;
_LazyAllMatches$Dart.prototype.$implements$Object$Dart = 1;
_LazyAllMatches$Dart.$Constructor = function(_regexp, _str){
  Object.$Constructor.call(this);
}
;
_LazyAllMatches$Dart.$Initializer = function(_regexp, _str){
  Object.$Initializer.call(this);
  this._regexp$$field_ = _regexp;
  this._str$$field_ = _str;
}
;
_LazyAllMatches$Dart._LazyAllMatches$$Factory = function(_regexp, _str){
  var tmp$0 = new _LazyAllMatches$Dart;
  tmp$0.$typeInfo = _LazyAllMatches$Dart.$lookupRTT();
  _LazyAllMatches$Dart.$Initializer.call(tmp$0, _regexp, _str);
  _LazyAllMatches$Dart.$Constructor.call(tmp$0, _regexp, _str);
  return tmp$0;
}
;
_LazyAllMatches$Dart.prototype._regexp$$named_ = function(){
  return this._regexp$$getter_().apply(this, arguments);
}
;
_LazyAllMatches$Dart.prototype._regexp$$getter_ = function(){
  return this._regexp$$field_;
}
;
_LazyAllMatches$Dart.prototype._regexp$$setter_ = function(tmp$0){
  this._regexp$$field_ = tmp$0;
}
;
_LazyAllMatches$Dart.prototype._str$$named_ = function(){
  return this._str$$getter_().apply(this, arguments);
}
;
_LazyAllMatches$Dart.prototype._str$$getter_ = function(){
  return this._str$$field_;
}
;
_LazyAllMatches$Dart.prototype._str$$setter_ = function(tmp$0){
  this._str$$field_ = tmp$0;
}
;
_LazyAllMatches$Dart.prototype.forEach$member = function(f){
  {
    var $0 = this.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var match_0 = $0.next$named(0, $noargs);
      {
        f(1, $noargs, match_0);
      }
    }
  }
}
;
_LazyAllMatches$Dart.prototype.forEach$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _LazyAllMatches$Dart.prototype.forEach$member.call(this, f);
}
;
_LazyAllMatches$Dart.prototype.forEach$getter = function forEach$getter(){
  return $bind(_LazyAllMatches$Dart.prototype.forEach$named, this);
}
;
_LazyAllMatches$Dart.prototype.filter$member = function(f){
  var result = ArrayFactory$Dart.Array$$Factory([Match$Dart.$lookupRTT()], $Dart$Null);
  {
    var $0 = this.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var match_0 = $0.next$named(0, $noargs);
      {
        if (f(1, $noargs, match_0)) {
          result.add$named(1, $noargs, match_0);
        }
      }
    }
  }
  return result;
}
;
_LazyAllMatches$Dart.prototype.filter$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _LazyAllMatches$Dart.prototype.filter$member.call(this, f);
}
;
_LazyAllMatches$Dart.prototype.filter$getter = function filter$getter(){
  return $bind(_LazyAllMatches$Dart.prototype.filter$named, this);
}
;
_LazyAllMatches$Dart.prototype.every$member = function(f){
  {
    var $0 = this.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var match_0 = $0.next$named(0, $noargs);
      {
        if (!f(1, $noargs, match_0)) {
          return false;
        }
      }
    }
  }
  return true;
}
;
_LazyAllMatches$Dart.prototype.every$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _LazyAllMatches$Dart.prototype.every$member.call(this, f);
}
;
_LazyAllMatches$Dart.prototype.every$getter = function every$getter(){
  return $bind(_LazyAllMatches$Dart.prototype.every$named, this);
}
;
_LazyAllMatches$Dart.prototype.some$member = function(f){
  {
    var $0 = this.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var match_0 = $0.next$named(0, $noargs);
      {
        if (f(1, $noargs, match_0)) {
          return true;
        }
      }
    }
  }
  return false;
}
;
_LazyAllMatches$Dart.prototype.some$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _LazyAllMatches$Dart.prototype.some$member.call(this, f);
}
;
_LazyAllMatches$Dart.prototype.some$getter = function some$getter(){
  return $bind(_LazyAllMatches$Dart.prototype.some$named, this);
}
;
_LazyAllMatches$Dart.prototype.isEmpty$member = function(){
  return EQ$operator(this._regexp$$getter_().firstMatch$named(1, $noargs, this._str$$getter_()), $Dart$Null);
}
;
_LazyAllMatches$Dart.prototype.isEmpty$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return _LazyAllMatches$Dart.prototype.isEmpty$member.call(this);
}
;
_LazyAllMatches$Dart.prototype.isEmpty$getter = function isEmpty$getter(){
  return $bind(_LazyAllMatches$Dart.prototype.isEmpty$named, this);
}
;
_LazyAllMatches$Dart.prototype.length$named = function(){
  return this.length$getter().apply(this, arguments);
}
;
_LazyAllMatches$Dart.prototype.length$getter = function(){
  var tmp$0;
  var result = 0;
  {
    var $1 = this.iterator$named(0, $noargs);
    while ($1.hasNext$named(0, $noargs)) {
      var match = $1.next$named(0, $noargs);
      {
        tmp$0 = result , (result = ADD$operator(tmp$0, 1) , tmp$0);
      }
    }
  }
  return result;
}
;
_LazyAllMatches$Dart.prototype.iterator$member = function(){
  return _LazyAllMatchesIterator$Dart._LazyAllMatchesIterator$$Factory(this._regexp$$getter_(), this._str$$getter_());
}
;
_LazyAllMatches$Dart.prototype.iterator$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return _LazyAllMatches$Dart.prototype.iterator$member.call(this);
}
;
_LazyAllMatches$Dart.prototype.iterator$getter = function iterator$getter(){
  return $bind(_LazyAllMatches$Dart.prototype.iterator$named, this);
}
;
_LazyAllMatches$Dart.prototype.$const_id = function(){
  return $cls('_LazyAllMatches$Dart') + (':' + $dart_const_id(this._regexp$$field_)) + (':' + $dart_const_id(this._str$$field_)) + (':' + $dart_const_id(this.length$field));
}
;
function _LazyAllMatchesIterator$Dart(){
}

_LazyAllMatchesIterator$Dart.$lookupRTT = function(){
  return RTT.create($cls('_LazyAllMatchesIterator$Dart'), _LazyAllMatchesIterator$Dart.$RTTimplements);
}
;
_LazyAllMatchesIterator$Dart.$RTTimplements = function(rtt){
  _LazyAllMatchesIterator$Dart.$addTo(rtt);
}
;
_LazyAllMatchesIterator$Dart.$addTo = function(target){
  var rtt = _LazyAllMatchesIterator$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Iterator$Dart.$addTo(target, [Match$Dart.$lookupRTT()]);
}
;
_LazyAllMatchesIterator$Dart.prototype.$implements$_LazyAllMatchesIterator$Dart = 1;
_LazyAllMatchesIterator$Dart.prototype.$implements$Iterator$Dart = 1;
_LazyAllMatchesIterator$Dart.prototype.$implements$Object$Dart = 1;
_LazyAllMatchesIterator$Dart.$Constructor = function(_regexp, _str){
  Object.$Constructor.call(this);
  this._jsInit$$member_(this._regexp$$getter_());
}
;
_LazyAllMatchesIterator$Dart.$Initializer = function(_regexp, _str){
  Object.$Initializer.call(this);
  this._regexp$$field_ = _regexp;
  this._str$$field_ = _str;
}
;
_LazyAllMatchesIterator$Dart._LazyAllMatchesIterator$$Factory = function(_regexp, _str){
  var tmp$0 = new _LazyAllMatchesIterator$Dart;
  tmp$0.$typeInfo = _LazyAllMatchesIterator$Dart.$lookupRTT();
  _LazyAllMatchesIterator$Dart.$Initializer.call(tmp$0, _regexp, _str);
  _LazyAllMatchesIterator$Dart.$Constructor.call(tmp$0, _regexp, _str);
  return tmp$0;
}
;
_LazyAllMatchesIterator$Dart.prototype._regexp$$named_ = function(){
  return this._regexp$$getter_().apply(this, arguments);
}
;
_LazyAllMatchesIterator$Dart.prototype._regexp$$getter_ = function(){
  return this._regexp$$field_;
}
;
_LazyAllMatchesIterator$Dart.prototype._regexp$$setter_ = function(tmp$0){
  this._regexp$$field_ = tmp$0;
}
;
_LazyAllMatchesIterator$Dart.prototype._str$$named_ = function(){
  return this._str$$getter_().apply(this, arguments);
}
;
_LazyAllMatchesIterator$Dart.prototype._str$$getter_ = function(){
  return this._str$$field_;
}
;
_LazyAllMatchesIterator$Dart.prototype._str$$setter_ = function(tmp$0){
  this._str$$field_ = tmp$0;
}
;
_LazyAllMatchesIterator$Dart.prototype._nextMatch$$named_ = function(){
  return this._nextMatch$$getter_().apply(this, arguments);
}
;
_LazyAllMatchesIterator$Dart.prototype._nextMatch$$getter_ = function(){
  return this._nextMatch$$field_;
}
;
_LazyAllMatchesIterator$Dart.prototype._nextMatch$$setter_ = function(tmp$0){
  this._nextMatch$$field_ = tmp$0;
}
;
_LazyAllMatchesIterator$Dart.prototype.next$member = function(){
  var tmp$0;
  if (!this.hasNext$member()) {
    $Dart$ThrowException($intern(NoMoreElementsException$Dart.NoMoreElementsException$$Factory()));
  }
  var result = this._nextMatch$$getter_();
  this._nextMatch$$setter_(tmp$0 = $Dart$Null) , tmp$0;
  return result;
}
;
_LazyAllMatchesIterator$Dart.prototype.next$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return _LazyAllMatchesIterator$Dart.prototype.next$member.call(this);
}
;
_LazyAllMatchesIterator$Dart.prototype.next$getter = function next$getter(){
  return $bind(_LazyAllMatchesIterator$Dart.prototype.next$named, this);
}
;
_LazyAllMatchesIterator$Dart.prototype.hasNext$member = function(){
  var tmp$0;
  if (NE$operator(this._nextMatch$$getter_(), $Dart$Null)) {
    return true;
  }
  this._nextMatch$$setter_(tmp$0 = this._computeNextMatch$$member_(this._regexp$$getter_(), this._str$$getter_())) , tmp$0;
  return NE$operator(this._nextMatch$$getter_(), $Dart$Null);
}
;
_LazyAllMatchesIterator$Dart.prototype.hasNext$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return _LazyAllMatchesIterator$Dart.prototype.hasNext$member.call(this);
}
;
_LazyAllMatchesIterator$Dart.prototype.hasNext$getter = function hasNext$getter(){
  return $bind(_LazyAllMatchesIterator$Dart.prototype.hasNext$named, this);
}
;
_LazyAllMatchesIterator$Dart.prototype._jsInit$$member_ = function(regexp){
  return native__LazyAllMatchesIterator__jsInit.call(this, regexp);
}
;
_LazyAllMatchesIterator$Dart.prototype._jsInit$$named_ = function($n, $o, regexp){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _LazyAllMatchesIterator$Dart.prototype._jsInit$$member_.call(this, regexp);
}
;
_LazyAllMatchesIterator$Dart.prototype._jsInit$$getter_ = function _jsInit$$getter_(){
  return $bind(_LazyAllMatchesIterator$Dart.prototype._jsInit$$named_, this);
}
;
_LazyAllMatchesIterator$Dart.prototype._computeNextMatch$$member_ = function(regexp, str){
  return native__LazyAllMatchesIterator__computeNextMatch.call(this, regexp, str);
}
;
_LazyAllMatchesIterator$Dart.prototype._computeNextMatch$$named_ = function($n, $o, regexp, str){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return _LazyAllMatchesIterator$Dart.prototype._computeNextMatch$$member_.call(this, regexp, str);
}
;
_LazyAllMatchesIterator$Dart.prototype._computeNextMatch$$getter_ = function _computeNextMatch$$getter_(){
  return $bind(_LazyAllMatchesIterator$Dart.prototype._computeNextMatch$$named_, this);
}
;
String.$lookupRTT = function(){
  return RTT.create($cls('String'), String.$RTTimplements);
}
;
String.$RTTimplements = function(rtt){
  String.$addTo(rtt);
}
;
String.$addTo = function(target){
  var rtt = String.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  String$Dart.$addTo(target);
}
;
String.prototype.$implements$StringImplementation$Dart = 1;
String.prototype.$implements$String$Dart = 1;
String.prototype.$implements$Comparable$Dart = 1;
String.prototype.$implements$Hashable$Dart = 1;
String.prototype.$implements$Pattern$Dart = 1;
String.prototype.$implements$Object$Dart = 1;
String.StringImplementation$fromValues$20$Factory = function(values){
  return String._newFromValues$$member_(values);
}
;
String.prototype.INDEX$operator = function(index){
  if (LTE$operator(0, index) && LT$operator(index, this.length$getter())) {
    return this._indexOperator$$member_(index);
  }
  $Dart$ThrowException(IndexOutOfRangeException$Dart.IndexOutOfRangeException$$Factory(index));
}
;
String.prototype.charCodeAt$member = function(index){
  if (LTE$operator(0, index) && LT$operator(index, this.length$getter())) {
    return this._charCodeAt$$member_(index);
  }
  $Dart$ThrowException(IndexOutOfRangeException$Dart.IndexOutOfRangeException$$Factory(index));
}
;
String.prototype.charCodeAt$named = function($n, $o, index){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return String.prototype.charCodeAt$member.call(this, index);
}
;
String.prototype.charCodeAt$getter = function charCodeAt$getter(){
  return $bind(String.prototype.charCodeAt$named, this);
}
;
String.prototype.length$named = function(){
  return this.length$getter().apply(this, arguments);
}
;
String.prototype.length$getter = function(){
  return native_StringImplementation_get$length.call(this);
}
;
String.prototype.EQ$operator = function(other){
  return native_StringImplementation_EQ.call(this, other);
}
;
String.prototype.substringMatches$member = function(start, other){
  var tmp$0;
  var len = this.length$getter();
  var otherLen = other.length$getter();
  if (EQ$operator(otherLen, 0)) {
    return true;
  }
  if (LT$operator(start, 0) || GTE$operator(start, len)) {
    return false;
  }
  if (GT$operator(ADD$operator(start, otherLen), len)) {
    return false;
  }
  var otherImpl = other;
  {
    var i = 0;
    for (; LT$operator(i, otherLen); tmp$0 = i , (i = ADD$operator(tmp$0, 1) , tmp$0)) {
      if (NE$operator(this._charCodeAt$$member_(ADD$operator(start, i)), otherImpl._charCodeAt$$named_(1, $noargs, i))) {
        return false;
      }
    }
  }
  return true;
}
;
String.prototype.substringMatches$named = function($n, $o, start, other){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return String.prototype.substringMatches$member.call(this, start, other);
}
;
String.prototype.substringMatches$getter = function substringMatches$getter(){
  return $bind(String.prototype.substringMatches$named, this);
}
;
String.prototype.endsWith$member = function(other){
  return this.substringMatches$member(SUB$operator(this.length$getter(), other.length$getter()), other);
}
;
String.prototype.endsWith$named = function($n, $o, other){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return String.prototype.endsWith$member.call(this, other);
}
;
String.prototype.endsWith$getter = function endsWith$getter(){
  return $bind(String.prototype.endsWith$named, this);
}
;
String.prototype.startsWith$member = function(other){
  return this.substringMatches$member(0, other);
}
;
String.prototype.startsWith$named = function($n, $o, other){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return String.prototype.startsWith$member.call(this, other);
}
;
String.prototype.startsWith$getter = function startsWith$getter(){
  return $bind(String.prototype.startsWith$named, this);
}
;
String.prototype.indexOf$member = function(other, startIndex){
  return native_StringImplementation_indexOf.call(this, other, startIndex);
}
;
String.prototype.indexOf$named = function($n, $o, other, startIndex){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return String.prototype.indexOf$member.call(this, other, startIndex);
}
;
String.prototype.indexOf$getter = function indexOf$getter(){
  return $bind(String.prototype.indexOf$named, this);
}
;
String.prototype.lastIndexOf$member = function(other, fromIndex){
  return native_StringImplementation_lastIndexOf.call(this, other, fromIndex);
}
;
String.prototype.lastIndexOf$named = function($n, $o, other, fromIndex){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return String.prototype.lastIndexOf$member.call(this, other, fromIndex);
}
;
String.prototype.lastIndexOf$getter = function lastIndexOf$getter(){
  return $bind(String.prototype.lastIndexOf$named, this);
}
;
String.prototype.isEmpty$member = function(){
  return EQ$operator(this.length$getter(), 0);
}
;
String.prototype.isEmpty$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return String.prototype.isEmpty$member.call(this);
}
;
String.prototype.isEmpty$getter = function isEmpty$getter(){
  return $bind(String.prototype.isEmpty$named, this);
}
;
String.prototype.concat$member = function(other){
  return native_StringImplementation_concat.call(this, other);
}
;
String.prototype.concat$named = function($n, $o, other){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return String.prototype.concat$member.call(this, other);
}
;
String.prototype.concat$getter = function concat$getter(){
  return $bind(String.prototype.concat$named, this);
}
;
String.prototype.ADD$operator = function(obj){
  return this.concat$named(1, $noargs, obj.toString$named(0, $noargs));
}
;
String.prototype.substring$member = function(startIndex, endIndex){
  if (EQ$operator(endIndex, $Dart$Null)) {
    endIndex = this.length$getter();
  }
  if (LT$operator(startIndex, 0) || GT$operator(startIndex, this.length$getter())) {
    $Dart$ThrowException(IndexOutOfRangeException$Dart.IndexOutOfRangeException$$Factory(startIndex));
  }
  if (LT$operator(endIndex, 0) || GT$operator(endIndex, this.length$getter())) {
    $Dart$ThrowException(IndexOutOfRangeException$Dart.IndexOutOfRangeException$$Factory(endIndex));
  }
  if (GT$operator(startIndex, endIndex)) {
    $Dart$ThrowException(IndexOutOfRangeException$Dart.IndexOutOfRangeException$$Factory(startIndex));
  }
  return this._substringUnchecked$$member_(startIndex, endIndex);
}
;
String.prototype.substring$named = function($n, $o, startIndex, endIndex){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 1:
      endIndex = $o.endIndex?(++seen , $o.endIndex):(++def , $Dart$Null);
  }
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return String.prototype.substring$member.call(this, startIndex, endIndex);
}
;
String.prototype.substring$getter = function substring$getter(){
  return $bind(String.prototype.substring$named, this);
}
;
String.prototype.trim$member = function(){
  return native_StringImplementation_trim.call(this);
}
;
String.prototype.trim$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return String.prototype.trim$member.call(this);
}
;
String.prototype.trim$getter = function trim$getter(){
  return $bind(String.prototype.trim$named, this);
}
;
String.prototype.contains$member = function(pattern, startIndex){
  var tmp$0;
  if (EQ$operator(startIndex, $Dart$Null)) {
    startIndex = 0;
  }
  if (LT$operator(startIndex, 0) || GT$operator(startIndex, this.length$getter())) {
    $Dart$ThrowException(IndexOutOfRangeException$Dart.IndexOutOfRangeException$$Factory(startIndex));
  }
  if (String.$instanceOf(pattern)) {
    return NE$operator(this.indexOf$named(2, $noargs, pattern, startIndex), negate$operator(1));
  }
   else {
    if (!!(tmp$0 = pattern , tmp$0 != null && tmp$0.$implements$JSSyntaxRegExp$Dart)) {
      var regExp = pattern;
      return regExp.hasMatch$named(1, $noargs, this._substringUnchecked$$member_(startIndex, this.length$getter()));
    }
     else {
      var substr = this._substringUnchecked$$member_(startIndex, this.length$getter());
      return !pattern.allMatches$named(1, $noargs, substr).iterator$named(0, $noargs).hasNext$named(0, $noargs);
    }
  }
}
;
String.prototype.contains$named = function($n, $o, pattern, startIndex){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return String.prototype.contains$member.call(this, pattern, startIndex);
}
;
String.prototype.contains$getter = function contains$getter(){
  return $bind(String.prototype.contains$named, this);
}
;
String.prototype.replaceFirst$member = function(from, to){
  var tmp$0;
  if (String.$instanceOf(from) || !!(tmp$0 = from , tmp$0 != null && tmp$0.$implements$JSSyntaxRegExp$Dart)) {
    return this._replace$$member_(from, to);
  }
   else {
    $Dart$ThrowException('StringImplementation.replace(Pattern) UNIMPLEMENTED');
  }
}
;
String.prototype.replaceFirst$named = function($n, $o, from, to){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return String.prototype.replaceFirst$member.call(this, from, to);
}
;
String.prototype.replaceFirst$getter = function replaceFirst$getter(){
  return $bind(String.prototype.replaceFirst$named, this);
}
;
String.prototype.replaceAll$member = function(from, to){
  var tmp$1, tmp$0;
  if (String.$instanceOf(from)) {
    if (EQ$operator(from, '')) {
      if (EQ$operator(this, '')) {
        return to;
      }
       else {
        var result = StringBufferImpl$Dart.StringBufferImpl$$Factory('');
        var len = this.length$getter();
        result.add$named(1, $noargs, to);
        {
          var i = 0;
          for (; LT$operator(i, len); tmp$0 = i , (i = ADD$operator(tmp$0, 1) , tmp$0)) {
            result.add$named(1, $noargs, this.INDEX$operator(i));
            result.add$named(1, $noargs, to);
          }
        }
        return result.toString$named(0, $noargs);
      }
    }
     else {
      return this._replaceAll$$member_(from, to);
    }
  }
   else {
    if (!!(tmp$1 = from , tmp$1 != null && tmp$1.$implements$JSSyntaxRegExp$Dart)) {
      return this._replaceAll$$member_(from, to);
    }
     else {
      $Dart$ThrowException('StringImplementation.replaceAll(Pattern) UNIMPLEMENTED');
    }
  }
}
;
String.prototype.replaceAll$named = function($n, $o, from, to){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return String.prototype.replaceAll$member.call(this, from, to);
}
;
String.prototype.replaceAll$getter = function replaceAll$getter(){
  return $bind(String.prototype.replaceAll$named, this);
}
;
String.prototype.split$member = function(pattern){
  var tmp$0;
  if (String.$instanceOf(pattern) || !!(tmp$0 = pattern , tmp$0 != null && tmp$0.$implements$JSSyntaxRegExp$Dart)) {
    return this._split$$member_(pattern);
  }
   else {
    $Dart$ThrowException('StringImplementation.split(Pattern) UNIMPLEMENTED');
  }
}
;
String.prototype.split$named = function($n, $o, pattern){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return String.prototype.split$member.call(this, pattern);
}
;
String.prototype.split$getter = function split$getter(){
  return $bind(String.prototype.split$named, this);
}
;
String.prototype.allMatches$member = function(str){
  var result = RTT.setTypeInfo([], Array.$lookupRTT());
  if (this.isEmpty$named(0, $noargs)) {
    return result;
  }
  var length_0 = this.length$getter();
  var ix = 0;
  while (LT$operator(ix, str.length$getter())) {
    var foundIx = str.indexOf$named(2, $noargs, this, ix);
    if (LT$operator(foundIx, 0)) {
      break;
    }
    result.add$named(1, $noargs, _StringMatch$Dart._StringMatch$$Factory(foundIx, str, this));
    ix = ADD$operator(foundIx, length_0);
  }
  return result;
}
;
String.prototype.allMatches$named = function($n, $o, str){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return String.prototype.allMatches$member.call(this, str);
}
;
String.prototype.allMatches$getter = function allMatches$getter(){
  return $bind(String.prototype.allMatches$named, this);
}
;
String.prototype.splitChars$member = function(){
  return this._split$$member_('');
}
;
String.prototype.splitChars$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return String.prototype.splitChars$member.call(this);
}
;
String.prototype.splitChars$getter = function splitChars$getter(){
  return $bind(String.prototype.splitChars$named, this);
}
;
String.prototype.charCodes$member = function(){
  var tmp$1, tmp$0;
  var len = this.length$getter();
  var result = ArrayFactory$Dart.Array$$Factory([int$Dart.$lookupRTT()], len);
  {
    var i = 0;
    for (; LT$operator(i, len); tmp$0 = i , (i = ADD$operator(tmp$0, 1) , tmp$0)) {
      result.ASSIGN_INDEX$operator(i, tmp$1 = this._charCodeAt$$member_(i)) , tmp$1;
    }
  }
  return result;
}
;
String.prototype.charCodes$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return String.prototype.charCodes$member.call(this);
}
;
String.prototype.charCodes$getter = function charCodes$getter(){
  return $bind(String.prototype.charCodes$named, this);
}
;
String.prototype.toLowerCase$member = function(){
  return native_StringImplementation_toLowerCase.call(this);
}
;
String.prototype.toLowerCase$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return String.prototype.toLowerCase$member.call(this);
}
;
String.prototype.toLowerCase$getter = function toLowerCase$getter(){
  return $bind(String.prototype.toLowerCase$named, this);
}
;
String.prototype.toUpperCase$member = function(){
  return native_StringImplementation_toUpperCase.call(this);
}
;
String.prototype.toUpperCase$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return String.prototype.toUpperCase$member.call(this);
}
;
String.prototype.toUpperCase$getter = function toUpperCase$getter(){
  return $bind(String.prototype.toUpperCase$named, this);
}
;
String.prototype.hashCode$member = function(){
  return native_StringImplementation_hashCode.call(this);
}
;
String.prototype.hashCode$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return String.prototype.hashCode$member.call(this);
}
;
String.prototype.hashCode$getter = function hashCode$getter(){
  return $bind(String.prototype.hashCode$named, this);
}
;
String.prototype.toString$member = function(){
  return native_StringImplementation_toString.call(this);
}
;
String.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return String.prototype.toString$member.call(this);
}
;
String.prototype.toString$getter = function toString$getter(){
  return $bind(String.prototype.toString$named, this);
}
;
String.prototype.compareTo$member = function(other){
  return native_StringImplementation_compareTo.call(this, other);
}
;
String.prototype.compareTo$named = function($n, $o, other){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return String.prototype.compareTo$member.call(this, other);
}
;
String.prototype.compareTo$getter = function compareTo$getter(){
  return $bind(String.prototype.compareTo$named, this);
}
;
String._newFromValues$$member_ = function(values){
  return native_StringImplementation__newFromValues(values);
}
;
String._newFromValues$$named_ = function($n, $o, values){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return String._newFromValues$$member_(values);
}
;
String._newFromValues$$getter_ = function _newFromValues$$getter_(){
  return String._newFromValues$$named_;
}
;
String.prototype._indexOperator$$member_ = function(index){
  return native_StringImplementation__indexOperator.call(this, index);
}
;
String.prototype._indexOperator$$named_ = function($n, $o, index){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return String.prototype._indexOperator$$member_.call(this, index);
}
;
String.prototype._indexOperator$$getter_ = function _indexOperator$$getter_(){
  return $bind(String.prototype._indexOperator$$named_, this);
}
;
String.prototype._charCodeAt$$member_ = function(index){
  return native_StringImplementation__charCodeAt.call(this, index);
}
;
String.prototype._charCodeAt$$named_ = function($n, $o, index){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return String.prototype._charCodeAt$$member_.call(this, index);
}
;
String.prototype._charCodeAt$$getter_ = function _charCodeAt$$getter_(){
  return $bind(String.prototype._charCodeAt$$named_, this);
}
;
String.prototype._substringUnchecked$$member_ = function(startIndex, endIndex){
  return native_StringImplementation__substringUnchecked.call(this, startIndex, endIndex);
}
;
String.prototype._substringUnchecked$$named_ = function($n, $o, startIndex, endIndex){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return String.prototype._substringUnchecked$$member_.call(this, startIndex, endIndex);
}
;
String.prototype._substringUnchecked$$getter_ = function _substringUnchecked$$getter_(){
  return $bind(String.prototype._substringUnchecked$$named_, this);
}
;
String.prototype._replace$$member_ = function(from, to){
  return native_StringImplementation__replace.call(this, from, to);
}
;
String.prototype._replace$$named_ = function($n, $o, from, to){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return String.prototype._replace$$member_.call(this, from, to);
}
;
String.prototype._replace$$getter_ = function _replace$$getter_(){
  return $bind(String.prototype._replace$$named_, this);
}
;
String.prototype._replaceAll$$member_ = function(from, to){
  return native_StringImplementation__replaceAll.call(this, from, to);
}
;
String.prototype._replaceAll$$named_ = function($n, $o, from, to){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return String.prototype._replaceAll$$member_.call(this, from, to);
}
;
String.prototype._replaceAll$$getter_ = function _replaceAll$$getter_(){
  return $bind(String.prototype._replaceAll$$named_, this);
}
;
String.prototype._split$$member_ = function(pattern){
  return native_StringImplementation__split.call(this, pattern);
}
;
String.prototype._split$$named_ = function($n, $o, pattern){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return String.prototype._split$$member_.call(this, pattern);
}
;
String.prototype._split$$getter_ = function _split$$getter_(){
  return $bind(String.prototype._split$$named_, this);
}
;
function _StringJsUtil$Dart(){
}

_StringJsUtil$Dart.$lookupRTT = function(){
  return RTT.create($cls('_StringJsUtil$Dart'));
}
;
_StringJsUtil$Dart.$addTo = function(target){
  var rtt = _StringJsUtil$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
_StringJsUtil$Dart.prototype.$implements$_StringJsUtil$Dart = 1;
_StringJsUtil$Dart.prototype.$implements$Object$Dart = 1;
_StringJsUtil$Dart.toDartString$member = function(o){
  if (o == null) {
    return 'null';
  }
  return o.toString$named(0, $noargs);
}
;
_StringJsUtil$Dart.toDartString$named = function($n, $o, o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _StringJsUtil$Dart.toDartString$member(o);
}
;
function native__StringJsUtil_toDartString(o){
  return _StringJsUtil$Dart.toDartString$member(o);
}

_StringJsUtil$Dart.toDartString$getter = function toDartString$getter(){
  return _StringJsUtil$Dart.toDartString$named;
}
;
_StringJsUtil$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
_StringJsUtil$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
_StringJsUtil$Dart._StringJsUtil$$Factory = function(){
  var tmp$0 = new _StringJsUtil$Dart;
  tmp$0.$typeInfo = _StringJsUtil$Dart.$lookupRTT();
  _StringJsUtil$Dart.$Initializer.call(tmp$0);
  _StringJsUtil$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function _StringMatch$Dart(){
}

_StringMatch$Dart.$lookupRTT = function(){
  return RTT.create($cls('_StringMatch$Dart'), _StringMatch$Dart.$RTTimplements);
}
;
_StringMatch$Dart.$RTTimplements = function(rtt){
  _StringMatch$Dart.$addTo(rtt);
}
;
_StringMatch$Dart.$addTo = function(target){
  var rtt = _StringMatch$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Match$Dart.$addTo(target);
}
;
_StringMatch$Dart.prototype.$implements$_StringMatch$Dart = 1;
_StringMatch$Dart.prototype.$implements$Match$Dart = 1;
_StringMatch$Dart.prototype.$implements$Object$Dart = 1;
_StringMatch$Dart.$Constructor = function(_start, str, pattern){
  Object.$Constructor.call(this);
}
;
_StringMatch$Dart.$Initializer = function(_start, str, pattern){
  Object.$Initializer.call(this);
  this._start$$field_ = _start;
  this.str$field = str;
  this.pattern$field = pattern;
}
;
_StringMatch$Dart._StringMatch$$Factory = function(_start, str, pattern){
  var tmp$0 = new _StringMatch$Dart;
  tmp$0.$typeInfo = _StringMatch$Dart.$lookupRTT();
  _StringMatch$Dart.$Initializer.call(tmp$0, _start, str, pattern);
  _StringMatch$Dart.$Constructor.call(tmp$0, _start, str, pattern);
  return tmp$0;
}
;
_StringMatch$Dart.prototype.start$member = function(){
  return this._start$$getter_();
}
;
_StringMatch$Dart.prototype.start$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return _StringMatch$Dart.prototype.start$member.call(this);
}
;
_StringMatch$Dart.prototype.start$getter = function start$getter(){
  return $bind(_StringMatch$Dart.prototype.start$named, this);
}
;
_StringMatch$Dart.prototype.end$member = function(){
  return ADD$operator(this._start$$getter_(), this.pattern$getter().length$getter());
}
;
_StringMatch$Dart.prototype.end$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return _StringMatch$Dart.prototype.end$member.call(this);
}
;
_StringMatch$Dart.prototype.end$getter = function end$getter(){
  return $bind(_StringMatch$Dart.prototype.end$named, this);
}
;
_StringMatch$Dart.prototype.INDEX$operator = function(g){
  return this.group$member(g);
}
;
_StringMatch$Dart.prototype.groupCount$member = function(){
  return 0;
}
;
_StringMatch$Dart.prototype.groupCount$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return _StringMatch$Dart.prototype.groupCount$member.call(this);
}
;
_StringMatch$Dart.prototype.groupCount$getter = function groupCount$getter(){
  return $bind(_StringMatch$Dart.prototype.groupCount$named, this);
}
;
_StringMatch$Dart.prototype.group$member = function(group){
  if (NE$operator(group, 0)) {
    $Dart$ThrowException(IndexOutOfRangeException$Dart.IndexOutOfRangeException$$Factory(group));
  }
  return this.pattern$getter();
}
;
_StringMatch$Dart.prototype.group$named = function($n, $o, group){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _StringMatch$Dart.prototype.group$member.call(this, group);
}
;
_StringMatch$Dart.prototype.group$getter = function group$getter(){
  return $bind(_StringMatch$Dart.prototype.group$named, this);
}
;
_StringMatch$Dart.prototype.groups$member = function(groups){
  var result = ArrayFactory$Dart.Array$$Factory([String$Dart.$lookupRTT()], $Dart$Null);
  {
    var $0 = groups.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var g = $0.next$named(0, $noargs);
      {
        result.add$named(1, $noargs, this.group$member(g));
      }
    }
  }
  return result;
}
;
_StringMatch$Dart.prototype.groups$named = function($n, $o, groups){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _StringMatch$Dart.prototype.groups$member.call(this, groups);
}
;
_StringMatch$Dart.prototype.groups$getter = function groups$getter(){
  return $bind(_StringMatch$Dart.prototype.groups$named, this);
}
;
_StringMatch$Dart.prototype._start$$named_ = function(){
  return this._start$$getter_().apply(this, arguments);
}
;
_StringMatch$Dart.prototype._start$$getter_ = function(){
  return this._start$$field_;
}
;
_StringMatch$Dart.prototype.str$named = function(){
  return this.str$getter().apply(this, arguments);
}
;
_StringMatch$Dart.prototype.str$getter = function(){
  return this.str$field;
}
;
_StringMatch$Dart.prototype.pattern$named = function(){
  return this.pattern$getter().apply(this, arguments);
}
;
_StringMatch$Dart.prototype.pattern$getter = function(){
  return this.pattern$field;
}
;
_StringMatch$Dart.prototype.$const_id = function(){
  return $cls('_StringMatch$Dart') + (':' + $dart_const_id(this._start$$field_)) + (':' + $dart_const_id(this.str$field)) + (':' + $dart_const_id(this.pattern$field));
}
;
function StringBase$Dart(){
}

StringBase$Dart.$lookupRTT = function(){
  return RTT.create($cls('StringBase$Dart'));
}
;
StringBase$Dart.$addTo = function(target){
  var rtt = StringBase$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
StringBase$Dart.prototype.$implements$StringBase$Dart = 1;
StringBase$Dart.prototype.$implements$Object$Dart = 1;
StringBase$Dart.createFromCharCodes$member = function(charCodes){
  return native_StringBase_createFromCharCodes(charCodes);
}
;
StringBase$Dart.createFromCharCodes$named = function($n, $o, charCodes){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return StringBase$Dart.createFromCharCodes$member(charCodes);
}
;
StringBase$Dart.createFromCharCodes$getter = function createFromCharCodes$getter(){
  return StringBase$Dart.createFromCharCodes$named;
}
;
StringBase$Dart.join$member = function(strings, separator){
  var tmp$0;
  var s = '';
  {
    var i = 0;
    for (; LT$operator(i, strings.length$getter()); tmp$0 = i , (i = ADD$operator(tmp$0, 1) , tmp$0)) {
      if (GT$operator(i, 0)) {
        s = s.concat$named(1, $noargs, separator);
      }
      s = s.concat$named(1, $noargs, strings.INDEX$operator(i));
    }
  }
  return s;
}
;
StringBase$Dart.join$named = function($n, $o, strings, separator){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return StringBase$Dart.join$member(strings, separator);
}
;
StringBase$Dart.join$getter = function join$getter(){
  return StringBase$Dart.join$named;
}
;
StringBase$Dart.concatAll$member = function(strings){
  return StringBase$Dart.join$member(strings, '');
}
;
StringBase$Dart.concatAll$named = function($n, $o, strings){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return StringBase$Dart.concatAll$member(strings);
}
;
StringBase$Dart.concatAll$getter = function concatAll$getter(){
  return StringBase$Dart.concatAll$named;
}
;
StringBase$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
StringBase$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
StringBase$Dart.StringBase$$Factory = function(){
  var tmp$0 = new StringBase$Dart;
  tmp$0.$typeInfo = StringBase$Dart.$lookupRTT();
  StringBase$Dart.$Initializer.call(tmp$0);
  StringBase$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function StringBufferImpl$Dart(){
}

StringBufferImpl$Dart.$lookupRTT = function(){
  return RTT.create($cls('StringBufferImpl$Dart'), StringBufferImpl$Dart.$RTTimplements);
}
;
StringBufferImpl$Dart.$RTTimplements = function(rtt){
  StringBufferImpl$Dart.$addTo(rtt);
}
;
StringBufferImpl$Dart.$addTo = function(target){
  var rtt = StringBufferImpl$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  StringBuffer$Dart.$addTo(target);
}
;
StringBufferImpl$Dart.prototype.$implements$StringBufferImpl$Dart = 1;
StringBufferImpl$Dart.prototype.$implements$StringBuffer$Dart = 1;
StringBufferImpl$Dart.prototype.$implements$Object$Dart = 1;
StringBufferImpl$Dart.$Constructor = function(content_0){
  Object.$Constructor.call(this);
  this.clear$member();
  this.add$member(content_0);
}
;
StringBufferImpl$Dart.$Initializer = function(content_0){
  Object.$Initializer.call(this);
}
;
StringBufferImpl$Dart.StringBufferImpl$$Factory = function(content_0){
  var tmp$0 = new StringBufferImpl$Dart;
  tmp$0.$typeInfo = StringBufferImpl$Dart.$lookupRTT();
  StringBufferImpl$Dart.$Initializer.call(tmp$0, content_0);
  StringBufferImpl$Dart.$Constructor.call(tmp$0, content_0);
  return tmp$0;
}
;
StringBufferImpl$Dart.prototype.length$named = function(){
  return this.length$getter().apply(this, arguments);
}
;
StringBufferImpl$Dart.prototype.length$getter = function(){
  return this._length$$getter_();
}
;
StringBufferImpl$Dart.prototype.isEmpty$member = function(){
  return EQ$operator(this._length$$getter_(), 0);
}
;
StringBufferImpl$Dart.prototype.isEmpty$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return StringBufferImpl$Dart.prototype.isEmpty$member.call(this);
}
;
StringBufferImpl$Dart.prototype.isEmpty$getter = function isEmpty$getter(){
  return $bind(StringBufferImpl$Dart.prototype.isEmpty$named, this);
}
;
StringBufferImpl$Dart.prototype.add$member = function(obj){
  var tmp$0;
  var str = obj.toString$named(0, $noargs);
  if (str == null || str.isEmpty$named(0, $noargs)) {
    return this;
  }
  this._buffer$$getter_().add$named(1, $noargs, str);
  this._length$$setter_(tmp$0 = ADD$operator(this._length$$getter_(), str.length$getter())) , tmp$0;
  return this;
}
;
StringBufferImpl$Dart.prototype.add$named = function($n, $o, obj){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return StringBufferImpl$Dart.prototype.add$member.call(this, obj);
}
;
StringBufferImpl$Dart.prototype.add$getter = function add$getter(){
  return $bind(StringBufferImpl$Dart.prototype.add$named, this);
}
;
StringBufferImpl$Dart.prototype.addAll$member = function(objects){
  {
    var $0 = objects.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var obj = $0.next$named(0, $noargs);
      {
        this.add$member(obj);
      }
    }
  }
  return this;
}
;
StringBufferImpl$Dart.prototype.addAll$named = function($n, $o, objects){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return StringBufferImpl$Dart.prototype.addAll$member.call(this, objects);
}
;
StringBufferImpl$Dart.prototype.addAll$getter = function addAll$getter(){
  return $bind(StringBufferImpl$Dart.prototype.addAll$named, this);
}
;
StringBufferImpl$Dart.prototype.addCharCode$member = function(charCode){
  return this.add$member(Strings$Dart.String$fromCharCodes$6$Factory(RTT.setTypeInfo([charCode], Array.$lookupRTT())));
}
;
StringBufferImpl$Dart.prototype.addCharCode$named = function($n, $o, charCode){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return StringBufferImpl$Dart.prototype.addCharCode$member.call(this, charCode);
}
;
StringBufferImpl$Dart.prototype.addCharCode$getter = function addCharCode$getter(){
  return $bind(StringBufferImpl$Dart.prototype.addCharCode$named, this);
}
;
StringBufferImpl$Dart.prototype.clear$member = function(){
  var tmp$1, tmp$0;
  this._buffer$$setter_(tmp$0 = ArrayFactory$Dart.Array$$Factory([String$Dart.$lookupRTT()], $Dart$Null)) , tmp$0;
  this._length$$setter_(tmp$1 = 0) , tmp$1;
  return this;
}
;
StringBufferImpl$Dart.prototype.clear$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return StringBufferImpl$Dart.prototype.clear$member.call(this);
}
;
StringBufferImpl$Dart.prototype.clear$getter = function clear$getter(){
  return $bind(StringBufferImpl$Dart.prototype.clear$named, this);
}
;
StringBufferImpl$Dart.prototype.toString$member = function(){
  if (EQ$operator(this._buffer$$getter_().length$getter(), 0)) {
    return '';
  }
  if (EQ$operator(this._buffer$$getter_().length$getter(), 1)) {
    return this._buffer$$getter_().INDEX$operator(0);
  }
  var result = StringBase$Dart.concatAll$member(this._buffer$$getter_());
  this._buffer$$getter_().clear$named(0, $noargs);
  this._buffer$$getter_().add$named(1, $noargs, result);
  return result;
}
;
StringBufferImpl$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return StringBufferImpl$Dart.prototype.toString$member.call(this);
}
;
StringBufferImpl$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(StringBufferImpl$Dart.prototype.toString$named, this);
}
;
StringBufferImpl$Dart.prototype._buffer$$named_ = function(){
  return this._buffer$$getter_().apply(this, arguments);
}
;
StringBufferImpl$Dart.prototype._buffer$$getter_ = function(){
  return this._buffer$$field_;
}
;
StringBufferImpl$Dart.prototype._buffer$$setter_ = function(tmp$0){
  this._buffer$$field_ = tmp$0;
}
;
StringBufferImpl$Dart.prototype._length$$named_ = function(){
  return this._length$$getter_().apply(this, arguments);
}
;
StringBufferImpl$Dart.prototype._length$$getter_ = function(){
  return this._length$$field_;
}
;
StringBufferImpl$Dart.prototype._length$$setter_ = function(tmp$0){
  this._length$$field_ = tmp$0;
}
;
function TimeZoneImplementation$Dart(){
}

TimeZoneImplementation$Dart.$lookupRTT = function(){
  return RTT.create($cls('TimeZoneImplementation$Dart'), TimeZoneImplementation$Dart.$RTTimplements);
}
;
TimeZoneImplementation$Dart.$RTTimplements = function(rtt){
  TimeZoneImplementation$Dart.$addTo(rtt);
}
;
TimeZoneImplementation$Dart.$addTo = function(target){
  var rtt = TimeZoneImplementation$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  TimeZone$Dart.$addTo(target);
}
;
TimeZoneImplementation$Dart.prototype.$implements$TimeZoneImplementation$Dart = 1;
TimeZoneImplementation$Dart.prototype.$implements$TimeZone$Dart = 1;
TimeZoneImplementation$Dart.prototype.$implements$Object$Dart = 1;
TimeZoneImplementation$Dart.utc$Constructor = function(){
  Object.$Constructor.call(this);
}
;
TimeZoneImplementation$Dart.utc$Initializer = function(){
  Object.$Initializer.call(this);
  this.isUtc$field = true;
}
;
TimeZoneImplementation$Dart.TimeZoneImplementation$utc$22$Factory = function(){
  var tmp$0 = new TimeZoneImplementation$Dart;
  tmp$0.$typeInfo = TimeZoneImplementation$Dart.$lookupRTT();
  TimeZoneImplementation$Dart.utc$Initializer.call(tmp$0);
  TimeZoneImplementation$Dart.utc$Constructor.call(tmp$0);
  return tmp$0;
}
;
TimeZoneImplementation$Dart.local$Constructor = function(){
  Object.$Constructor.call(this);
}
;
TimeZoneImplementation$Dart.local$Initializer = function(){
  Object.$Initializer.call(this);
  this.isUtc$field = false;
}
;
TimeZoneImplementation$Dart.TimeZoneImplementation$local$22$Factory = function(){
  var tmp$0 = new TimeZoneImplementation$Dart;
  tmp$0.$typeInfo = TimeZoneImplementation$Dart.$lookupRTT();
  TimeZoneImplementation$Dart.local$Initializer.call(tmp$0);
  TimeZoneImplementation$Dart.local$Constructor.call(tmp$0);
  return tmp$0;
}
;
TimeZoneImplementation$Dart.prototype.EQ$operator = function(other){
  var tmp$0;
  if (!!!(tmp$0 = other , tmp$0 != null && tmp$0.$implements$TimeZoneImplementation$Dart)) {
    return false;
  }
  return EQ$operator(this.isUtc$getter(), other.isUtc$getter());
}
;
TimeZoneImplementation$Dart.prototype.toString$member = function(){
  if (this.isUtc$getter()) {
    return 'TimeZone (UTC)';
  }
  return 'TimeZone (Local)';
}
;
TimeZoneImplementation$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return TimeZoneImplementation$Dart.prototype.toString$member.call(this);
}
;
TimeZoneImplementation$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(TimeZoneImplementation$Dart.prototype.toString$named, this);
}
;
TimeZoneImplementation$Dart.prototype.isUtc$named = function(){
  return this.isUtc$getter().apply(this, arguments);
}
;
TimeZoneImplementation$Dart.prototype.isUtc$getter = function(){
  return this.isUtc$field;
}
;
TimeZoneImplementation$Dart.prototype.$const_id = function(){
  return $cls('TimeZoneImplementation$Dart') + (':' + $dart_const_id(this.isUtc$field));
}
;
function TypeToken$Dart(){
}

TypeToken$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('TypeToken$Dart'), null, typeArgs);
}
;
TypeToken$Dart.$addTo = function(target, typeArgs){
  var rtt = TypeToken$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
}
;
TypeToken$Dart.prototype.$implements$TypeToken$Dart = 1;
TypeToken$Dart.prototype.$implements$Object$Dart = 1;
TypeToken$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
TypeToken$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
TypeToken$Dart.TypeToken$$Factory = function($rtt){
  var tmp$0 = new TypeToken$Dart;
  tmp$0.$typeInfo = $rtt;
  TypeToken$Dart.$Initializer.call(tmp$0);
  TypeToken$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
TypeToken$Dart.prototype.$const_id = function(){
  return $cls('TypeToken$Dart');
}
;
function Array$Dart(){
}

Array$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('Array$Dart'), Array$Dart.$RTTimplements, typeArgs);
}
;
Array$Dart.$RTTimplements = function(rtt, typeArgs){
  Array$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
Array$Dart.$addTo = function(target, typeArgs){
  var rtt = Array$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  List$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0)]);
}
;
function DualPivotQuicksort$Dart(){
}

DualPivotQuicksort$Dart.$lookupRTT = function(){
  return RTT.create($cls('DualPivotQuicksort$Dart'));
}
;
DualPivotQuicksort$Dart.$addTo = function(target){
  var rtt = DualPivotQuicksort$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
DualPivotQuicksort$Dart.prototype.$implements$DualPivotQuicksort$Dart = 1;
DualPivotQuicksort$Dart.prototype.$implements$Object$Dart = 1;
DualPivotQuicksort$Dart._INSERTION_SORT_THRESHOLD$$named_ = function(){
  return DualPivotQuicksort$Dart._INSERTION_SORT_THRESHOLD$$getter_().apply(this, arguments);
}
;
DualPivotQuicksort$Dart._INSERTION_SORT_THRESHOLD$$getter_ = function(){
  return 32;
}
;
DualPivotQuicksort$Dart.sort$member = function(a, compare){
  DualPivotQuicksort$Dart._doSort$$member_(a, 0, SUB$operator(a.length$getter(), 1), compare);
}
;
DualPivotQuicksort$Dart.sort$named = function($n, $o, a, compare){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return DualPivotQuicksort$Dart.sort$member(a, compare);
}
;
DualPivotQuicksort$Dart.sort$getter = function sort$getter(){
  return DualPivotQuicksort$Dart.sort$named;
}
;
DualPivotQuicksort$Dart.sortRange$member = function(a, from, to, compare){
  if (LT$operator(from, 0) || GT$operator(to, a.length$getter()) || LT$operator(to, from)) {
    $Dart$ThrowException('OutOfRange');
  }
  DualPivotQuicksort$Dart._doSort$$member_(a, from, SUB$operator(to, 1), compare);
}
;
DualPivotQuicksort$Dart.sortRange$named = function($n, $o, a, from, to, compare){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 4)
    $nsme();
  return DualPivotQuicksort$Dart.sortRange$member(a, from, to, compare);
}
;
DualPivotQuicksort$Dart.sortRange$getter = function sortRange$getter(){
  return DualPivotQuicksort$Dart.sortRange$named;
}
;
DualPivotQuicksort$Dart._doSort$$member_ = function(a, left, right, compare){
  if (LTE$operator(SUB$operator(right, left), DualPivotQuicksort$Dart._INSERTION_SORT_THRESHOLD$$getter_())) {
    DualPivotQuicksort$Dart.insertionSort_$member(a, left, right, compare);
  }
   else {
    DualPivotQuicksort$Dart._dualPivotQuicksort$$member_(a, left, right, compare);
  }
}
;
DualPivotQuicksort$Dart._doSort$$named_ = function($n, $o, a, left, right, compare){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 4)
    $nsme();
  return DualPivotQuicksort$Dart._doSort$$member_(a, left, right, compare);
}
;
DualPivotQuicksort$Dart._doSort$$getter_ = function _doSort$$getter_(){
  return DualPivotQuicksort$Dart._doSort$$named_;
}
;
DualPivotQuicksort$Dart.insertionSort_$member = function(a, left, right, compare){
  var tmp$1, tmp$2, tmp$3, tmp$0;
  {
    var i = ADD$operator(left, 1);
    for (; LTE$operator(i, right); tmp$0 = i , (i = ADD$operator(tmp$0, 1) , tmp$0)) {
      var el = a.INDEX$operator(i);
      var j = i;
      while (GT$operator(j, left) && GT$operator(compare(2, $noargs, a.INDEX$operator(SUB$operator(j, 1)), el), 0)) {
        a.ASSIGN_INDEX$operator(j, tmp$1 = a.INDEX$operator(SUB$operator(j, 1))) , tmp$1;
        tmp$2 = j , (j = SUB$operator(tmp$2, 1) , tmp$2);
      }
      a.ASSIGN_INDEX$operator(j, tmp$3 = el) , tmp$3;
    }
  }
}
;
DualPivotQuicksort$Dart.insertionSort_$named = function($n, $o, a, left, right, compare){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 4)
    $nsme();
  return DualPivotQuicksort$Dart.insertionSort_$member(a, left, right, compare);
}
;
DualPivotQuicksort$Dart.insertionSort_$getter = function insertionSort_$getter(){
  return DualPivotQuicksort$Dart.insertionSort_$named;
}
;
DualPivotQuicksort$Dart._dualPivotQuicksort$$member_ = function(a, left, right, compare){
  var tmp$48, tmp$47, tmp$49, tmp$9, tmp$43, tmp$44, tmp$45, tmp$46, tmp$5, tmp$6, tmp$40, tmp$7, tmp$41, tmp$8, tmp$42, tmp$1, tmp$2, tmp$3, tmp$4, tmp$0, tmp$39, tmp$38, tmp$37, tmp$36, tmp$34, tmp$35, tmp$32, tmp$33, tmp$30, tmp$31, tmp$20, tmp$24, tmp$23, tmp$22, tmp$21, tmp$27, tmp$28, tmp$25, tmp$26, tmp$29, tmp$11, tmp$10, tmp$13, tmp$12, tmp$14, tmp$15, tmp$16, tmp$17, tmp$18, tmp$19;
  assert(GT$operator(SUB$operator(right, left), DualPivotQuicksort$Dart._INSERTION_SORT_THRESHOLD$$getter_()));
  var sixth = TRUNC$operator(ADD$operator(SUB$operator(right, left), 1), 6);
  var index1 = ADD$operator(left, sixth);
  var index2 = ADD$operator(index1, sixth);
  var index3 = SUB$operator(right, sixth);
  var index4 = SUB$operator(index3, sixth);
  var index5 = TRUNC$operator(ADD$operator(left, right), 2);
  var el1 = a.INDEX$operator(index1);
  var el2 = a.INDEX$operator(index2);
  var el3 = a.INDEX$operator(index3);
  var el4 = a.INDEX$operator(index4);
  var el5 = a.INDEX$operator(index5);
  if (GT$operator(compare(2, $noargs, el1, el2), 0)) {
    var t = el1;
    el1 = el2;
    el2 = t;
  }
  if (GT$operator(compare(2, $noargs, el4, el5), 0)) {
    var t_0 = el4;
    el4 = el5;
    el5 = t_0;
  }
  if (GT$operator(compare(2, $noargs, el1, el3), 0)) {
    var t_0_0 = el1;
    el1 = el3;
    el3 = t_0_0;
  }
  if (GT$operator(compare(2, $noargs, el2, el3), 0)) {
    var t_1 = el2;
    el2 = el3;
    el3 = t_1;
  }
  if (GT$operator(compare(2, $noargs, el1, el4), 0)) {
    var t_1_1 = el1;
    el1 = el4;
    el4 = t_1_1;
  }
  if (GT$operator(compare(2, $noargs, el3, el4), 0)) {
    var t_2 = el3;
    el3 = el4;
    el4 = t_2;
  }
  if (GT$operator(compare(2, $noargs, el2, el5), 0)) {
    var t_2_2 = el2;
    el2 = el5;
    el5 = t_2_2;
  }
  if (GT$operator(compare(2, $noargs, el2, el3), 0)) {
    var t_3 = el2;
    el2 = el3;
    el3 = t_3;
  }
  if (GT$operator(compare(2, $noargs, el4, el5), 0)) {
    var t_3_3 = el4;
    el4 = el5;
    el5 = t_3_3;
  }
  var pivot1 = el2;
  var pivot2 = el4;
  a.ASSIGN_INDEX$operator(index1, tmp$0 = el1) , tmp$0;
  a.ASSIGN_INDEX$operator(index3, tmp$1 = el3) , tmp$1;
  a.ASSIGN_INDEX$operator(index5, tmp$2 = el5) , tmp$2;
  a.ASSIGN_INDEX$operator(index2, tmp$3 = a.INDEX$operator(left)) , tmp$3;
  a.ASSIGN_INDEX$operator(index4, tmp$4 = a.INDEX$operator(right)) , tmp$4;
  var less = ADD$operator(left, 1);
  var great = SUB$operator(right, 1);
  var pivots_are_equal = EQ$operator(compare(2, $noargs, pivot1, pivot2), 0);
  if (pivots_are_equal) {
    var pivot = pivot1;
    {
      var k = less;
      for (; LTE$operator(k, great); tmp$5 = k , (k = ADD$operator(tmp$5, 1) , tmp$5)) {
        var ak = a.INDEX$operator(k);
        var comp = compare(2, $noargs, ak, pivot);
        if (EQ$operator(comp, 0)) {
          continue;
        }
        if (LT$operator(comp, 0)) {
          if (NE$operator(k, less)) {
            a.ASSIGN_INDEX$operator(k, tmp$6 = a.INDEX$operator(less)) , tmp$6;
            a.ASSIGN_INDEX$operator(less, tmp$7 = ak) , tmp$7;
          }
          tmp$8 = less , (less = ADD$operator(tmp$8, 1) , tmp$8);
        }
         else {
          while (true) {
            var comp_4 = compare(2, $noargs, a.INDEX$operator(great), pivot);
            if (LT$operator(comp_4, 0)) {
              tmp$9 = great , (great = SUB$operator(tmp$9, 1) , tmp$9);
              continue;
            }
             else {
              if (EQ$operator(comp_4, 0)) {
                a.ASSIGN_INDEX$operator(k, tmp$10 = a.INDEX$operator(less)) , tmp$10;
                a.ASSIGN_INDEX$operator((tmp$11 = less , (less = ADD$operator(tmp$11, 1) , tmp$11)), tmp$12 = a.INDEX$operator(great)) , tmp$12;
                a.ASSIGN_INDEX$operator((tmp$13 = great , (great = SUB$operator(tmp$13, 1) , tmp$13)), tmp$14 = ak) , tmp$14;
                break;
              }
               else {
                a.ASSIGN_INDEX$operator(k, tmp$15 = a.INDEX$operator(great)) , tmp$15;
                a.ASSIGN_INDEX$operator((tmp$16 = great , (great = SUB$operator(tmp$16, 1) , tmp$16)), tmp$17 = ak) , tmp$17;
                break;
              }
            }
          }
        }
      }
    }
  }
   else {
    {
      var k_4 = less;
      for (; LTE$operator(k_4, great); tmp$18 = k_4 , (k_4 = ADD$operator(tmp$18, 1) , tmp$18)) {
        var ak_4 = a.INDEX$operator(k_4);
        var comp_pivot1 = compare(2, $noargs, ak_4, pivot1);
        if (LT$operator(comp_pivot1, 0)) {
          if (NE$operator(k_4, less)) {
            a.ASSIGN_INDEX$operator(k_4, tmp$19 = a.INDEX$operator(less)) , tmp$19;
            a.ASSIGN_INDEX$operator(less, tmp$20 = ak_4) , tmp$20;
          }
          tmp$21 = less , (less = ADD$operator(tmp$21, 1) , tmp$21);
        }
         else {
          var comp_pivot2 = compare(2, $noargs, ak_4, pivot2);
          if (GT$operator(comp_pivot2, 0)) {
            while (true) {
              var comp_4_4 = compare(2, $noargs, a.INDEX$operator(great), pivot2);
              if (GT$operator(comp_4_4, 0)) {
                tmp$22 = great , (great = SUB$operator(tmp$22, 1) , tmp$22);
                if (LT$operator(great, k_4)) {
                  break;
                }
                continue;
              }
               else {
                var comp_5 = compare(2, $noargs, a.INDEX$operator(great), pivot1);
                if (LT$operator(comp_5, 0)) {
                  a.ASSIGN_INDEX$operator(k_4, tmp$23 = a.INDEX$operator(less)) , tmp$23;
                  a.ASSIGN_INDEX$operator((tmp$24 = less , (less = ADD$operator(tmp$24, 1) , tmp$24)), tmp$25 = a.INDEX$operator(great)) , tmp$25;
                  a.ASSIGN_INDEX$operator((tmp$26 = great , (great = SUB$operator(tmp$26, 1) , tmp$26)), tmp$27 = ak_4) , tmp$27;
                }
                 else {
                  a.ASSIGN_INDEX$operator(k_4, tmp$28 = a.INDEX$operator(great)) , tmp$28;
                  a.ASSIGN_INDEX$operator((tmp$29 = great , (great = SUB$operator(tmp$29, 1) , tmp$29)), tmp$30 = ak_4) , tmp$30;
                }
                break;
              }
            }
          }
        }
      }
    }
  }
  a.ASSIGN_INDEX$operator(left, tmp$31 = a.INDEX$operator(SUB$operator(less, 1))) , tmp$31;
  a.ASSIGN_INDEX$operator(SUB$operator(less, 1), tmp$32 = pivot1) , tmp$32;
  a.ASSIGN_INDEX$operator(right, tmp$33 = a.INDEX$operator(ADD$operator(great, 1))) , tmp$33;
  a.ASSIGN_INDEX$operator(ADD$operator(great, 1), tmp$34 = pivot2) , tmp$34;
  DualPivotQuicksort$Dart._doSort$$member_(a, left, SUB$operator(less, 2), compare);
  DualPivotQuicksort$Dart._doSort$$member_(a, ADD$operator(great, 2), right, compare);
  if (pivots_are_equal) {
    return;
  }
  if (LT$operator(less, index1) && GT$operator(great, index5)) {
    while (EQ$operator(compare(2, $noargs, a.INDEX$operator(less), pivot1), 0)) {
      tmp$35 = less , (less = ADD$operator(tmp$35, 1) , tmp$35);
    }
    while (EQ$operator(compare(2, $noargs, a.INDEX$operator(great), pivot2), 0)) {
      tmp$36 = great , (great = SUB$operator(tmp$36, 1) , tmp$36);
    }
    {
      var k_5 = less;
      for (; LTE$operator(k_5, great); tmp$37 = k_5 , (k_5 = ADD$operator(tmp$37, 1) , tmp$37)) {
        var ak_5 = a.INDEX$operator(k_5);
        var comp_pivot1_5 = compare(2, $noargs, ak_5, pivot1);
        if (EQ$operator(comp_pivot1_5, 0)) {
          if (NE$operator(k_5, less)) {
            a.ASSIGN_INDEX$operator(k_5, tmp$38 = a.INDEX$operator(less)) , tmp$38;
            a.ASSIGN_INDEX$operator(less, tmp$39 = ak_5) , tmp$39;
          }
          tmp$40 = less , (less = ADD$operator(tmp$40, 1) , tmp$40);
        }
         else {
          var comp_pivot2_5 = compare(2, $noargs, ak_5, pivot2);
          if (EQ$operator(comp_pivot2_5, 0)) {
            while (true) {
              var comp_5_5 = compare(2, $noargs, a.INDEX$operator(great), pivot2);
              if (EQ$operator(comp_5_5, 0)) {
                tmp$41 = great , (great = SUB$operator(tmp$41, 1) , tmp$41);
                if (LT$operator(great, k_5)) {
                  break;
                }
                continue;
              }
               else {
                var comp_6 = compare(2, $noargs, a.INDEX$operator(great), pivot1);
                if (EQ$operator(comp_6, 0)) {
                  a.ASSIGN_INDEX$operator(k_5, tmp$42 = a.INDEX$operator(less)) , tmp$42;
                  a.ASSIGN_INDEX$operator((tmp$43 = less , (less = ADD$operator(tmp$43, 1) , tmp$43)), tmp$44 = a.INDEX$operator(great)) , tmp$44;
                  a.ASSIGN_INDEX$operator((tmp$45 = great , (great = SUB$operator(tmp$45, 1) , tmp$45)), tmp$46 = ak_5) , tmp$46;
                }
                 else {
                  a.ASSIGN_INDEX$operator(k_5, tmp$47 = a.INDEX$operator(great)) , tmp$47;
                  a.ASSIGN_INDEX$operator((tmp$48 = great , (great = SUB$operator(tmp$48, 1) , tmp$48)), tmp$49 = ak_5) , tmp$49;
                }
                break;
              }
            }
          }
        }
      }
    }
    DualPivotQuicksort$Dart._doSort$$member_(a, less, great, compare);
  }
   else {
    DualPivotQuicksort$Dart._doSort$$member_(a, less, great, compare);
  }
}
;
DualPivotQuicksort$Dart._dualPivotQuicksort$$named_ = function($n, $o, a, left, right, compare){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 4)
    $nsme();
  return DualPivotQuicksort$Dart._dualPivotQuicksort$$member_(a, left, right, compare);
}
;
DualPivotQuicksort$Dart._dualPivotQuicksort$$getter_ = function _dualPivotQuicksort$$getter_(){
  return DualPivotQuicksort$Dart._dualPivotQuicksort$$named_;
}
;
DualPivotQuicksort$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
DualPivotQuicksort$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
DualPivotQuicksort$Dart.DualPivotQuicksort$$Factory = function(){
  var tmp$0 = new DualPivotQuicksort$Dart;
  tmp$0.$typeInfo = DualPivotQuicksort$Dart.$lookupRTT();
  DualPivotQuicksort$Dart.$Initializer.call(tmp$0);
  DualPivotQuicksort$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function DurationImplementation$Dart(){
}

DurationImplementation$Dart.$lookupRTT = function(){
  return RTT.create($cls('DurationImplementation$Dart'), DurationImplementation$Dart.$RTTimplements);
}
;
DurationImplementation$Dart.$RTTimplements = function(rtt){
  DurationImplementation$Dart.$addTo(rtt);
}
;
DurationImplementation$Dart.$addTo = function(target){
  var rtt = DurationImplementation$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Duration$Dart.$addTo(target);
}
;
DurationImplementation$Dart.prototype.$implements$DurationImplementation$Dart = 1;
DurationImplementation$Dart.prototype.$implements$Duration$Dart = 1;
DurationImplementation$Dart.prototype.$implements$Comparable$Dart = 1;
DurationImplementation$Dart.prototype.$implements$Object$Dart = 1;
DurationImplementation$Dart.$Constructor = function(days, hours, minutes, seconds, milliseconds){
  Object.$Constructor.call(this);
}
;
DurationImplementation$Dart.$Initializer = function(days, hours, minutes, seconds, milliseconds){
  Object.$Initializer.call(this);
  this._durationInMilliseconds$$field_ = ADD$operator(ADD$operator(ADD$operator(ADD$operator(MUL$operator(days, Duration$Dart.MILLISECONDS_PER_DAY$getter()), MUL$operator(hours, Duration$Dart.MILLISECONDS_PER_HOUR$getter())), MUL$operator(minutes, Duration$Dart.MILLISECONDS_PER_MINUTE$getter())), MUL$operator(seconds, Duration$Dart.MILLISECONDS_PER_SECOND$getter())), milliseconds);
}
;
DurationImplementation$Dart.DurationImplementation$$Factory = function(days, hours, minutes, seconds, milliseconds){
  var tmp$0 = new DurationImplementation$Dart;
  tmp$0.$typeInfo = DurationImplementation$Dart.$lookupRTT();
  DurationImplementation$Dart.$Initializer.call(tmp$0, days, hours, minutes, seconds, milliseconds);
  DurationImplementation$Dart.$Constructor.call(tmp$0, days, hours, minutes, seconds, milliseconds);
  return tmp$0;
}
;
DurationImplementation$Dart.prototype._durationInMilliseconds$$named_ = function(){
  return this._durationInMilliseconds$$getter_().apply(this, arguments);
}
;
DurationImplementation$Dart.prototype._durationInMilliseconds$$getter_ = function(){
  return this._durationInMilliseconds$$field_;
}
;
DurationImplementation$Dart.prototype.inDays$named = function(){
  return this.inDays$getter().apply(this, arguments);
}
;
DurationImplementation$Dart.prototype.inDays$getter = function(){
  return TRUNC$operator(this._durationInMilliseconds$$getter_(), Duration$Dart.MILLISECONDS_PER_DAY$getter());
}
;
DurationImplementation$Dart.prototype.inHours$named = function(){
  return this.inHours$getter().apply(this, arguments);
}
;
DurationImplementation$Dart.prototype.inHours$getter = function(){
  return TRUNC$operator(this._durationInMilliseconds$$getter_(), Duration$Dart.MILLISECONDS_PER_HOUR$getter());
}
;
DurationImplementation$Dart.prototype.inMinutes$named = function(){
  return this.inMinutes$getter().apply(this, arguments);
}
;
DurationImplementation$Dart.prototype.inMinutes$getter = function(){
  return TRUNC$operator(this._durationInMilliseconds$$getter_(), Duration$Dart.MILLISECONDS_PER_MINUTE$getter());
}
;
DurationImplementation$Dart.prototype.inSeconds$named = function(){
  return this.inSeconds$getter().apply(this, arguments);
}
;
DurationImplementation$Dart.prototype.inSeconds$getter = function(){
  return TRUNC$operator(this._durationInMilliseconds$$getter_(), Duration$Dart.MILLISECONDS_PER_SECOND$getter());
}
;
DurationImplementation$Dart.prototype.inMilliseconds$named = function(){
  return this.inMilliseconds$getter().apply(this, arguments);
}
;
DurationImplementation$Dart.prototype.inMilliseconds$getter = function(){
  return this._durationInMilliseconds$$getter_();
}
;
DurationImplementation$Dart.prototype.EQ$operator = function(other){
  var tmp$0;
  if (!!!(tmp$0 = other , tmp$0 != null && tmp$0.$implements$DurationImplementation$Dart)) {
    return false;
  }
  return EQ$operator(this._durationInMilliseconds$$getter_(), other.inMilliseconds$getter());
}
;
DurationImplementation$Dart.prototype.hashCode$member = function(){
  return this._durationInMilliseconds$$getter_().hashCode$named(0, $noargs);
}
;
DurationImplementation$Dart.prototype.hashCode$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DurationImplementation$Dart.prototype.hashCode$member.call(this);
}
;
DurationImplementation$Dart.prototype.hashCode$getter = function hashCode$getter(){
  return $bind(DurationImplementation$Dart.prototype.hashCode$named, this);
}
;
DurationImplementation$Dart.prototype.compareTo$member = function(other){
  return this.inMilliseconds$getter().compareTo$named(1, $noargs, other.inMilliseconds$getter());
}
;
DurationImplementation$Dart.prototype.compareTo$named = function($n, $o, other){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DurationImplementation$Dart.prototype.compareTo$member.call(this, other);
}
;
DurationImplementation$Dart.prototype.compareTo$getter = function compareTo$getter(){
  return $bind(DurationImplementation$Dart.prototype.compareTo$named, this);
}
;
function DurationImplementation$Dart$toString$c0$threeDigits$27_8_2$Hoisted(n){
  if (GTE$operator(n, 100)) {
    return '' + $toString(n) + '';
  }
  if (GT$operator(n, 10)) {
    return '0' + $toString(n) + '';
  }
  return '00' + $toString(n) + '';
}

function DurationImplementation$Dart$toString$c0$threeDigits$27_8_2$Hoisted$named($n, $o, n){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DurationImplementation$Dart$toString$c0$threeDigits$27_8_2$Hoisted(n);
}

function DurationImplementation$Dart$toString$c1$twoDigits$27_8_2$Hoisted(n){
  if (GTE$operator(n, 10)) {
    return '' + $toString(n) + '';
  }
  return '0' + $toString(n) + '';
}

function DurationImplementation$Dart$toString$c1$twoDigits$27_8_2$Hoisted$named($n, $o, n){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DurationImplementation$Dart$toString$c1$twoDigits$27_8_2$Hoisted(n);
}

DurationImplementation$Dart.prototype.toString$member = function(){
  var threeDigits = $bind(DurationImplementation$Dart$toString$c0$threeDigits$27_8_2$Hoisted$named, $Dart$Null);
  var twoDigits = $bind(DurationImplementation$Dart$toString$c1$twoDigits$27_8_2$Hoisted$named, $Dart$Null);
  if (LT$operator(this._durationInMilliseconds$$getter_(), 0)) {
    var duration = DurationImplementation$Dart.DurationImplementation$$Factory(0, 0, 0, 0, negate$operator(this._durationInMilliseconds$$getter_()));
    return '-' + $toString(duration) + '';
  }
  var twoDigitMinutes = twoDigits(1, $noargs, this.inMinutes$getter().remainder$named(1, $noargs, Duration$Dart.MINUTES_PER_HOUR$getter()));
  var twoDigitSeconds = twoDigits(1, $noargs, this.inSeconds$getter().remainder$named(1, $noargs, Duration$Dart.SECONDS_PER_MINUTE$getter()));
  var threeDigitMs = threeDigits(1, $noargs, this.inMilliseconds$getter().remainder$named(1, $noargs, Duration$Dart.MILLISECONDS_PER_SECOND$getter()));
  return '' + $toString(this.inHours$getter()) + ':' + $toString(twoDigitMinutes) + ':' + $toString(twoDigitSeconds) + '.' + $toString(threeDigitMs) + '';
}
;
DurationImplementation$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DurationImplementation$Dart.prototype.toString$member.call(this);
}
;
DurationImplementation$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(DurationImplementation$Dart.prototype.toString$named, this);
}
;
DurationImplementation$Dart.prototype.$const_id = function(){
  return $cls('DurationImplementation$Dart') + (':' + $dart_const_id(this._durationInMilliseconds$$field_)) + (':' + $dart_const_id(this.inDays$field)) + (':' + $dart_const_id(this.inHours$field)) + (':' + $dart_const_id(this.inMinutes$field)) + (':' + $dart_const_id(this.inSeconds$field)) + (':' + $dart_const_id(this.inMilliseconds$field));
}
;
function ExceptionImplementation$Dart(){
}

ExceptionImplementation$Dart.$lookupRTT = function(){
  return RTT.create($cls('ExceptionImplementation$Dart'), ExceptionImplementation$Dart.$RTTimplements);
}
;
ExceptionImplementation$Dart.$RTTimplements = function(rtt){
  ExceptionImplementation$Dart.$addTo(rtt);
}
;
ExceptionImplementation$Dart.$addTo = function(target){
  var rtt = ExceptionImplementation$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
ExceptionImplementation$Dart.prototype.$implements$ExceptionImplementation$Dart = 1;
ExceptionImplementation$Dart.prototype.$implements$Exception$Dart = 1;
ExceptionImplementation$Dart.prototype.$implements$Object$Dart = 1;
ExceptionImplementation$Dart.$Constructor = function(_msg){
  Object.$Constructor.call(this);
}
;
ExceptionImplementation$Dart.$Initializer = function(_msg){
  Object.$Initializer.call(this);
  this._msg$$field_ = _msg;
}
;
ExceptionImplementation$Dart.ExceptionImplementation$$Factory = function(_msg){
  var tmp$0 = new ExceptionImplementation$Dart;
  tmp$0.$typeInfo = ExceptionImplementation$Dart.$lookupRTT();
  ExceptionImplementation$Dart.$Initializer.call(tmp$0, _msg);
  ExceptionImplementation$Dart.$Constructor.call(tmp$0, _msg);
  return tmp$0;
}
;
ExceptionImplementation$Dart.prototype.toString$member = function(){
  return this._msg$$getter_() == null?'Exception':'Exception: ' + $toString(this._msg$$getter_()) + '';
}
;
ExceptionImplementation$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return ExceptionImplementation$Dart.prototype.toString$member.call(this);
}
;
ExceptionImplementation$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(ExceptionImplementation$Dart.prototype.toString$named, this);
}
;
ExceptionImplementation$Dart.prototype._msg$$named_ = function(){
  return this._msg$$getter_().apply(this, arguments);
}
;
ExceptionImplementation$Dart.prototype._msg$$getter_ = function(){
  return this._msg$$field_;
}
;
ExceptionImplementation$Dart.prototype.$const_id = function(){
  return $cls('ExceptionImplementation$Dart') + (':' + $dart_const_id(this._msg$$field_));
}
;
function HashMapImplementation$Dart(){
}

HashMapImplementation$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('HashMapImplementation$Dart'), HashMapImplementation$Dart.$RTTimplements, typeArgs);
}
;
HashMapImplementation$Dart.$RTTimplements = function(rtt, typeArgs){
  HashMapImplementation$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
HashMapImplementation$Dart.$addTo = function(target, typeArgs){
  var rtt = HashMapImplementation$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  HashMap$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0), RTT.getTypeArg(target.typeArgs, 1)]);
}
;
HashMapImplementation$Dart.prototype.$implements$HashMapImplementation$Dart = 1;
HashMapImplementation$Dart.prototype.$implements$HashMap$Dart = 1;
HashMapImplementation$Dart.prototype.$implements$Map$Dart = 1;
HashMapImplementation$Dart.prototype.$implements$Object$Dart = 1;
HashMapImplementation$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
  var tmp$5, tmp$1, tmp$2, tmp$3, tmp$4, tmp$0;
  if (HashMapImplementation$Dart._deletedKey$$getter_() == null) {
    HashMapImplementation$Dart._deletedKey$$setter_(tmp$0 = Object.Object$$Factory()) , tmp$0;
  }
  this._numberOfEntries$$setter_(tmp$1 = 0) , tmp$1;
  this._numberOfDeleted$$setter_(tmp$2 = 0) , tmp$2;
  this._loadLimit$$setter_(tmp$3 = HashMapImplementation$Dart._computeLoadLimit$$member_(HashMapImplementation$Dart._INITIAL_CAPACITY$$getter_())) , tmp$3;
  this._keys$$setter_(tmp$4 = ListFactory$Dart.List$$Factory(null, HashMapImplementation$Dart._INITIAL_CAPACITY$$getter_())) , tmp$4;
  this._values$$setter_(tmp$5 = ListFactory$Dart.List$$Factory([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('HashMapImplementation$Dart')), 1)], HashMapImplementation$Dart._INITIAL_CAPACITY$$getter_())) , tmp$5;
}
;
HashMapImplementation$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
HashMapImplementation$Dart.HashMapImplementation$$Factory = function($rtt){
  var tmp$0 = new HashMapImplementation$Dart;
  tmp$0.$typeInfo = $rtt;
  HashMapImplementation$Dart.$Initializer.call(tmp$0);
  HashMapImplementation$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function HashMapImplementation$Dart$from$c0$26_26$HoistedConstructor(dartc_scp$1, key, value){
  var tmp$0;
  dartc_scp$1.result.ASSIGN_INDEX$operator(key, tmp$0 = value) , tmp$0;
}

function HashMapImplementation$Dart$from$c0$26_26$HoistedConstructor$named($s0, $n, $o, key, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return HashMapImplementation$Dart$from$c0$26_26$HoistedConstructor($s0, key, value);
}

HashMapImplementation$Dart.HashMapImplementation$from$21$Factory = function(other){
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.result = HashMapImplementation$Dart.HashMapImplementation$$Factory(HashMapImplementation$Dart.$lookupRTT());
  other.forEach$named(1, $noargs, $bind(HashMapImplementation$Dart$from$c0$26_26$HoistedConstructor$named, $Dart$Null, dartc_scp$1));
  return dartc_scp$1.result;
  dartc_scp$1 = $Dart$Null;
}
;
HashMapImplementation$Dart.prototype._keys$$named_ = function(){
  return this._keys$$getter_().apply(this, arguments);
}
;
HashMapImplementation$Dart.prototype._keys$$getter_ = function(){
  return this._keys$$field_;
}
;
HashMapImplementation$Dart.prototype._keys$$setter_ = function(tmp$0){
  this._keys$$field_ = tmp$0;
}
;
HashMapImplementation$Dart.prototype._values$$named_ = function(){
  return this._values$$getter_().apply(this, arguments);
}
;
HashMapImplementation$Dart.prototype._values$$getter_ = function(){
  return this._values$$field_;
}
;
HashMapImplementation$Dart.prototype._values$$setter_ = function(tmp$0){
  this._values$$field_ = tmp$0;
}
;
HashMapImplementation$Dart.prototype._loadLimit$$named_ = function(){
  return this._loadLimit$$getter_().apply(this, arguments);
}
;
HashMapImplementation$Dart.prototype._loadLimit$$getter_ = function(){
  return this._loadLimit$$field_;
}
;
HashMapImplementation$Dart.prototype._loadLimit$$setter_ = function(tmp$0){
  this._loadLimit$$field_ = tmp$0;
}
;
HashMapImplementation$Dart.prototype._numberOfEntries$$named_ = function(){
  return this._numberOfEntries$$getter_().apply(this, arguments);
}
;
HashMapImplementation$Dart.prototype._numberOfEntries$$getter_ = function(){
  return this._numberOfEntries$$field_;
}
;
HashMapImplementation$Dart.prototype._numberOfEntries$$setter_ = function(tmp$0){
  this._numberOfEntries$$field_ = tmp$0;
}
;
HashMapImplementation$Dart.prototype._numberOfDeleted$$named_ = function(){
  return this._numberOfDeleted$$getter_().apply(this, arguments);
}
;
HashMapImplementation$Dart.prototype._numberOfDeleted$$getter_ = function(){
  return this._numberOfDeleted$$field_;
}
;
HashMapImplementation$Dart.prototype._numberOfDeleted$$setter_ = function(tmp$0){
  this._numberOfDeleted$$field_ = tmp$0;
}
;
HashMapImplementation$Dart._deletedKey$$named_ = function(){
  return HashMapImplementation$Dart._deletedKey$$getter_().apply(this, arguments);
}
;
HashMapImplementation$Dart._deletedKey$$getter_ = function(){
  return isolate$current.HashMapImplementation$Dart_deletedKey$$field_;
}
;
HashMapImplementation$Dart._deletedKey$$setter_ = function(tmp$0){
  isolate$current.HashMapImplementation$Dart_deletedKey$$field_ = tmp$0;
}
;
HashMapImplementation$Dart._INITIAL_CAPACITY$$named_ = function(){
  return HashMapImplementation$Dart._INITIAL_CAPACITY$$getter_().apply(this, arguments);
}
;
HashMapImplementation$Dart._INITIAL_CAPACITY$$getter_ = function(){
  return 8;
}
;
HashMapImplementation$Dart._computeLoadLimit$$member_ = function(capacity){
  return TRUNC$operator(MUL$operator(capacity, 3), 4);
}
;
HashMapImplementation$Dart._computeLoadLimit$$named_ = function($n, $o, capacity){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashMapImplementation$Dart._computeLoadLimit$$member_(capacity);
}
;
HashMapImplementation$Dart._computeLoadLimit$$getter_ = function _computeLoadLimit$$getter_(){
  return HashMapImplementation$Dart._computeLoadLimit$$named_;
}
;
HashMapImplementation$Dart._firstProbe$$member_ = function(hashCode, length_0){
  return BIT_AND$operator(hashCode, SUB$operator(length_0, 1));
}
;
HashMapImplementation$Dart._firstProbe$$named_ = function($n, $o, hashCode, length_0){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return HashMapImplementation$Dart._firstProbe$$member_(hashCode, length_0);
}
;
HashMapImplementation$Dart._firstProbe$$getter_ = function _firstProbe$$getter_(){
  return HashMapImplementation$Dart._firstProbe$$named_;
}
;
HashMapImplementation$Dart._nextProbe$$member_ = function(currentProbe, numberOfProbes, length_0){
  return BIT_AND$operator(ADD$operator(currentProbe, numberOfProbes), SUB$operator(length_0, 1));
}
;
HashMapImplementation$Dart._nextProbe$$named_ = function($n, $o, currentProbe, numberOfProbes, length_0){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return HashMapImplementation$Dart._nextProbe$$member_(currentProbe, numberOfProbes, length_0);
}
;
HashMapImplementation$Dart._nextProbe$$getter_ = function _nextProbe$$getter_(){
  return HashMapImplementation$Dart._nextProbe$$named_;
}
;
HashMapImplementation$Dart.prototype._probeForAdding$$member_ = function(key){
  var tmp$0;
  var hash = HashMapImplementation$Dart._firstProbe$$member_(key.hashCode$named(0, $noargs), this._keys$$getter_().length$getter());
  var numberOfProbes = 1;
  var initialHash = hash;
  var insertionIndex = negate$operator(1);
  while (true) {
    var existingKey = this._keys$$getter_().INDEX$operator(hash);
    if (existingKey == null) {
      if (LT$operator(insertionIndex, 0)) {
        return hash;
      }
      return insertionIndex;
    }
     else {
      if (EQ$operator(existingKey, key)) {
        return hash;
      }
       else {
        if (LT$operator(insertionIndex, 0) && HashMapImplementation$Dart._deletedKey$$getter_() === existingKey) {
          insertionIndex = hash;
        }
      }
    }
    hash = HashMapImplementation$Dart._nextProbe$$member_(hash, (tmp$0 = numberOfProbes , (numberOfProbes = ADD$operator(tmp$0, 1) , tmp$0)), this._keys$$getter_().length$getter());
  }
}
;
HashMapImplementation$Dart.prototype._probeForAdding$$named_ = function($n, $o, key){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashMapImplementation$Dart.prototype._probeForAdding$$member_.call(this, key);
}
;
HashMapImplementation$Dart.prototype._probeForAdding$$getter_ = function _probeForAdding$$getter_(){
  return $bind(HashMapImplementation$Dart.prototype._probeForAdding$$named_, this);
}
;
HashMapImplementation$Dart.prototype._probeForLookup$$member_ = function(key){
  var tmp$0;
  var hash = HashMapImplementation$Dart._firstProbe$$member_(key.hashCode$named(0, $noargs), this._keys$$getter_().length$getter());
  var numberOfProbes = 1;
  var initialHash = hash;
  while (true) {
    var existingKey = this._keys$$getter_().INDEX$operator(hash);
    if (existingKey == null) {
      return negate$operator(1);
    }
    if (EQ$operator(existingKey, key)) {
      return hash;
    }
    hash = HashMapImplementation$Dart._nextProbe$$member_(hash, (tmp$0 = numberOfProbes , (numberOfProbes = ADD$operator(tmp$0, 1) , tmp$0)), this._keys$$getter_().length$getter());
  }
}
;
HashMapImplementation$Dart.prototype._probeForLookup$$named_ = function($n, $o, key){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashMapImplementation$Dart.prototype._probeForLookup$$member_.call(this, key);
}
;
HashMapImplementation$Dart.prototype._probeForLookup$$getter_ = function _probeForLookup$$getter_(){
  return $bind(HashMapImplementation$Dart.prototype._probeForLookup$$named_, this);
}
;
HashMapImplementation$Dart.prototype._ensureCapacity$$member_ = function(){
  var newNumberOfEntries = ADD$operator(this._numberOfEntries$$getter_(), 1);
  if (GTE$operator(newNumberOfEntries, this._loadLimit$$getter_())) {
    this._grow$$member_(MUL$operator(this._keys$$getter_().length$getter(), 2));
    return;
  }
  var capacity = this._keys$$getter_().length$getter();
  var numberOfFreeOrDeleted = SUB$operator(capacity, newNumberOfEntries);
  var numberOfFree = SUB$operator(numberOfFreeOrDeleted, this._numberOfDeleted$$getter_());
  if (GT$operator(this._numberOfDeleted$$getter_(), numberOfFree)) {
    this._grow$$member_(this._keys$$getter_().length$getter());
  }
}
;
HashMapImplementation$Dart.prototype._ensureCapacity$$named_ = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return HashMapImplementation$Dart.prototype._ensureCapacity$$member_.call(this);
}
;
HashMapImplementation$Dart.prototype._ensureCapacity$$getter_ = function _ensureCapacity$$getter_(){
  return $bind(HashMapImplementation$Dart.prototype._ensureCapacity$$named_, this);
}
;
HashMapImplementation$Dart._isPowerOfTwo$$member_ = function(x){
  return EQ$operator(BIT_AND$operator(x, SUB$operator(x, 1)), 0);
}
;
HashMapImplementation$Dart._isPowerOfTwo$$named_ = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashMapImplementation$Dart._isPowerOfTwo$$member_(x);
}
;
HashMapImplementation$Dart._isPowerOfTwo$$getter_ = function _isPowerOfTwo$$getter_(){
  return HashMapImplementation$Dart._isPowerOfTwo$$named_;
}
;
HashMapImplementation$Dart.prototype._grow$$member_ = function(newCapacity){
  var tmp$5, tmp$6, tmp$1, tmp$2, tmp$3, tmp$4, tmp$0;
  assert(HashMapImplementation$Dart._isPowerOfTwo$$member_(newCapacity));
  var capacity = this._keys$$getter_().length$getter();
  this._loadLimit$$setter_(tmp$0 = HashMapImplementation$Dart._computeLoadLimit$$member_(newCapacity)) , tmp$0;
  var oldKeys = this._keys$$getter_();
  var oldValues = this._values$$getter_();
  this._keys$$setter_(tmp$1 = ListFactory$Dart.List$$Factory(null, newCapacity)) , tmp$1;
  this._values$$setter_(tmp$2 = ListFactory$Dart.List$$Factory([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('HashMapImplementation$Dart')), 1)], newCapacity)) , tmp$2;
  {
    var i = 0;
    for (; LT$operator(i, capacity); tmp$3 = i , (i = ADD$operator(tmp$3, 1) , tmp$3)) {
      var key = oldKeys.INDEX$operator(i);
      if (key == null || key === HashMapImplementation$Dart._deletedKey$$getter_()) {
        continue;
      }
      var value = oldValues.INDEX$operator(i);
      var newIndex = this._probeForAdding$$member_(key);
      this._keys$$getter_().ASSIGN_INDEX$operator(newIndex, tmp$4 = key) , tmp$4;
      this._values$$getter_().ASSIGN_INDEX$operator(newIndex, tmp$5 = value) , tmp$5;
    }
  }
  this._numberOfDeleted$$setter_(tmp$6 = 0) , tmp$6;
}
;
HashMapImplementation$Dart.prototype._grow$$named_ = function($n, $o, newCapacity){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashMapImplementation$Dart.prototype._grow$$member_.call(this, newCapacity);
}
;
HashMapImplementation$Dart.prototype._grow$$getter_ = function _grow$$getter_(){
  return $bind(HashMapImplementation$Dart.prototype._grow$$named_, this);
}
;
HashMapImplementation$Dart.prototype.clear$member = function(){
  var tmp$1, tmp$2, tmp$3, tmp$4, tmp$0;
  this._numberOfEntries$$setter_(tmp$0 = 0) , tmp$0;
  this._numberOfDeleted$$setter_(tmp$1 = 0) , tmp$1;
  var length_0 = this._keys$$getter_().length$getter();
  {
    var i = 0;
    for (; LT$operator(i, length_0); tmp$2 = i , (i = ADD$operator(tmp$2, 1) , tmp$2)) {
      this._keys$$getter_().ASSIGN_INDEX$operator(i, tmp$3 = $Dart$Null) , tmp$3;
      this._values$$getter_().ASSIGN_INDEX$operator(i, tmp$4 = $Dart$Null) , tmp$4;
    }
  }
}
;
HashMapImplementation$Dart.prototype.clear$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return HashMapImplementation$Dart.prototype.clear$member.call(this);
}
;
HashMapImplementation$Dart.prototype.clear$getter = function clear$getter(){
  return $bind(HashMapImplementation$Dart.prototype.clear$named, this);
}
;
HashMapImplementation$Dart.prototype.ASSIGN_INDEX$operator = function(key, value){
  var tmp$1, tmp$2, tmp$3, tmp$0;
  this._ensureCapacity$$member_();
  var index = this._probeForAdding$$member_(key);
  if (this._keys$$getter_().INDEX$operator(index) == null || this._keys$$getter_().INDEX$operator(index) === HashMapImplementation$Dart._deletedKey$$getter_()) {
    tmp$0 = this._numberOfEntries$$getter_() , (this._numberOfEntries$$setter_(tmp$1 = ADD$operator(tmp$0, 1)) , tmp$1 , tmp$0);
  }
  this._keys$$getter_().ASSIGN_INDEX$operator(index, tmp$2 = key) , tmp$2;
  this._values$$getter_().ASSIGN_INDEX$operator(index, tmp$3 = value) , tmp$3;
}
;
HashMapImplementation$Dart.prototype.INDEX$operator = function(key){
  var index = this._probeForLookup$$member_(key);
  if (LT$operator(index, 0)) {
    return $Dart$Null;
  }
  return this._values$$getter_().INDEX$operator(index);
}
;
HashMapImplementation$Dart.prototype.putIfAbsent$member = function(key, ifAbsent){
  var tmp$0;
  var index = this._probeForLookup$$member_(key);
  if (GTE$operator(index, 0)) {
    return this._values$$getter_().INDEX$operator(index);
  }
  var value = ifAbsent(0, $noargs);
  this.ASSIGN_INDEX$operator(key, tmp$0 = value) , tmp$0;
  return value;
}
;
HashMapImplementation$Dart.prototype.putIfAbsent$named = function($n, $o, key, ifAbsent){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return HashMapImplementation$Dart.prototype.putIfAbsent$member.call(this, key, ifAbsent);
}
;
HashMapImplementation$Dart.prototype.putIfAbsent$getter = function putIfAbsent$getter(){
  return $bind(HashMapImplementation$Dart.prototype.putIfAbsent$named, this);
}
;
HashMapImplementation$Dart.prototype.remove$member = function(key){
  var tmp$5, tmp$1, tmp$2, tmp$3, tmp$4, tmp$0;
  var index = this._probeForLookup$$member_(key);
  if (GTE$operator(index, 0)) {
    tmp$0 = this._numberOfEntries$$getter_() , (this._numberOfEntries$$setter_(tmp$1 = SUB$operator(tmp$0, 1)) , tmp$1 , tmp$0);
    var value = this._values$$getter_().INDEX$operator(index);
    this._values$$getter_().ASSIGN_INDEX$operator(index, tmp$2 = $Dart$Null) , tmp$2;
    this._keys$$getter_().ASSIGN_INDEX$operator(index, tmp$3 = HashMapImplementation$Dart._deletedKey$$getter_()) , tmp$3;
    tmp$4 = this._numberOfDeleted$$getter_() , (this._numberOfDeleted$$setter_(tmp$5 = ADD$operator(tmp$4, 1)) , tmp$5 , tmp$4);
    return value;
  }
  return $Dart$Null;
}
;
HashMapImplementation$Dart.prototype.remove$named = function($n, $o, key){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashMapImplementation$Dart.prototype.remove$member.call(this, key);
}
;
HashMapImplementation$Dart.prototype.remove$getter = function remove$getter(){
  return $bind(HashMapImplementation$Dart.prototype.remove$named, this);
}
;
HashMapImplementation$Dart.prototype.isEmpty$member = function(){
  return EQ$operator(this._numberOfEntries$$getter_(), 0);
}
;
HashMapImplementation$Dart.prototype.isEmpty$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return HashMapImplementation$Dart.prototype.isEmpty$member.call(this);
}
;
HashMapImplementation$Dart.prototype.isEmpty$getter = function isEmpty$getter(){
  return $bind(HashMapImplementation$Dart.prototype.isEmpty$named, this);
}
;
HashMapImplementation$Dart.prototype.length$named = function(){
  return this.length$getter().apply(this, arguments);
}
;
HashMapImplementation$Dart.prototype.length$getter = function(){
  return this._numberOfEntries$$getter_();
}
;
HashMapImplementation$Dart.prototype.forEach$member = function(f){
  var tmp$0;
  var length_0 = this._keys$$getter_().length$getter();
  {
    var i = 0;
    for (; LT$operator(i, length_0); tmp$0 = i , (i = ADD$operator(tmp$0, 1) , tmp$0)) {
      if (this._keys$$getter_().INDEX$operator(i) != null && this._keys$$getter_().INDEX$operator(i) !== HashMapImplementation$Dart._deletedKey$$getter_()) {
        f(2, $noargs, this._keys$$getter_().INDEX$operator(i), this._values$$getter_().INDEX$operator(i));
      }
    }
  }
}
;
HashMapImplementation$Dart.prototype.forEach$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashMapImplementation$Dart.prototype.forEach$member.call(this, f);
}
;
HashMapImplementation$Dart.prototype.forEach$getter = function forEach$getter(){
  return $bind(HashMapImplementation$Dart.prototype.forEach$named, this);
}
;
function HashMapImplementation$Dart$getKeys$c0$_$26_7_2$Hoisted(dartc_scp$1, key, value){
  var tmp$1, tmp$0;
  dartc_scp$1.list.ASSIGN_INDEX$operator((tmp$0 = dartc_scp$1.i , (dartc_scp$1.i = ADD$operator(tmp$0, 1) , tmp$0)), tmp$1 = key) , tmp$1;
}

function HashMapImplementation$Dart$getKeys$c0$_$26_7_2$Hoisted$named($s0, $n, $o, key, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return HashMapImplementation$Dart$getKeys$c0$_$26_7_2$Hoisted($s0, key, value);
}

HashMapImplementation$Dart.prototype.getKeys$member = function(){
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.list = ListFactory$Dart.List$$Factory([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('HashMapImplementation$Dart')), 0)], this.length$getter());
  dartc_scp$1.i = 0;
  this.forEach$member($bind(HashMapImplementation$Dart$getKeys$c0$_$26_7_2$Hoisted$named, $Dart$Null, dartc_scp$1));
  return dartc_scp$1.list;
  dartc_scp$1 = $Dart$Null;
}
;
HashMapImplementation$Dart.prototype.getKeys$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return HashMapImplementation$Dart.prototype.getKeys$member.call(this);
}
;
HashMapImplementation$Dart.prototype.getKeys$getter = function getKeys$getter(){
  return $bind(HashMapImplementation$Dart.prototype.getKeys$named, this);
}
;
function HashMapImplementation$Dart$getValues$c0$_$26_9_2$Hoisted(dartc_scp$1, key, value){
  var tmp$1, tmp$0;
  dartc_scp$1.list.ASSIGN_INDEX$operator((tmp$0 = dartc_scp$1.i , (dartc_scp$1.i = ADD$operator(tmp$0, 1) , tmp$0)), tmp$1 = value) , tmp$1;
}

function HashMapImplementation$Dart$getValues$c0$_$26_9_2$Hoisted$named($s0, $n, $o, key, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return HashMapImplementation$Dart$getValues$c0$_$26_9_2$Hoisted($s0, key, value);
}

HashMapImplementation$Dart.prototype.getValues$member = function(){
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.list = ListFactory$Dart.List$$Factory([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('HashMapImplementation$Dart')), 1)], this.length$getter());
  dartc_scp$1.i = 0;
  this.forEach$member($bind(HashMapImplementation$Dart$getValues$c0$_$26_9_2$Hoisted$named, $Dart$Null, dartc_scp$1));
  return dartc_scp$1.list;
  dartc_scp$1 = $Dart$Null;
}
;
HashMapImplementation$Dart.prototype.getValues$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return HashMapImplementation$Dart.prototype.getValues$member.call(this);
}
;
HashMapImplementation$Dart.prototype.getValues$getter = function getValues$getter(){
  return $bind(HashMapImplementation$Dart.prototype.getValues$named, this);
}
;
HashMapImplementation$Dart.prototype.containsKey$member = function(key){
  return NE$operator(this._probeForLookup$$member_(key), negate$operator(1));
}
;
HashMapImplementation$Dart.prototype.containsKey$named = function($n, $o, key){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashMapImplementation$Dart.prototype.containsKey$member.call(this, key);
}
;
HashMapImplementation$Dart.prototype.containsKey$getter = function containsKey$getter(){
  return $bind(HashMapImplementation$Dart.prototype.containsKey$named, this);
}
;
HashMapImplementation$Dart.prototype.containsValue$member = function(value){
  var tmp$0;
  var length_0 = this._values$$getter_().length$getter();
  {
    var i = 0;
    for (; LT$operator(i, length_0); tmp$0 = i , (i = ADD$operator(tmp$0, 1) , tmp$0)) {
      if (this._keys$$getter_().INDEX$operator(i) != null && this._keys$$getter_().INDEX$operator(i) !== HashMapImplementation$Dart._deletedKey$$getter_()) {
        if (EQ$operator(this._values$$getter_().INDEX$operator(i), value)) {
          return true;
        }
      }
    }
  }
  return false;
}
;
HashMapImplementation$Dart.prototype.containsValue$named = function($n, $o, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashMapImplementation$Dart.prototype.containsValue$member.call(this, value);
}
;
HashMapImplementation$Dart.prototype.containsValue$getter = function containsValue$getter(){
  return $bind(HashMapImplementation$Dart.prototype.containsValue$named, this);
}
;
function HashSetImplementation$Dart(){
}

HashSetImplementation$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('HashSetImplementation$Dart'), HashSetImplementation$Dart.$RTTimplements, typeArgs);
}
;
HashSetImplementation$Dart.$RTTimplements = function(rtt, typeArgs){
  HashSetImplementation$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
HashSetImplementation$Dart.$addTo = function(target, typeArgs){
  var rtt = HashSetImplementation$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  HashSet$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0)]);
}
;
HashSetImplementation$Dart.prototype.$implements$HashSetImplementation$Dart = 1;
HashSetImplementation$Dart.prototype.$implements$HashSet$Dart = 1;
HashSetImplementation$Dart.prototype.$implements$Set$Dart = 1;
HashSetImplementation$Dart.prototype.$implements$Collection$Dart = 1;
HashSetImplementation$Dart.prototype.$implements$Iterable$Dart = 1;
HashSetImplementation$Dart.prototype.$implements$Object$Dart = 1;
HashSetImplementation$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
  var tmp$0;
  this._backingMap$$setter_(tmp$0 = HashMapImplementation$Dart.HashMapImplementation$$Factory(HashMapImplementation$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('HashSetImplementation$Dart')), 0), RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('HashSetImplementation$Dart')), 0)]))) , tmp$0;
}
;
HashSetImplementation$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
HashSetImplementation$Dart.HashSetImplementation$$Factory = function($rtt){
  var tmp$0 = new HashSetImplementation$Dart;
  tmp$0.$typeInfo = $rtt;
  HashSetImplementation$Dart.$Initializer.call(tmp$0);
  HashSetImplementation$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
HashSetImplementation$Dart.HashSetImplementation$from$21$Factory = function(other){
  var set = HashSetImplementation$Dart.HashSetImplementation$$Factory(HashSetImplementation$Dart.$lookupRTT());
  {
    var $0 = other.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var e = $0.next$named(0, $noargs);
      {
        set.add$named(1, $noargs, e);
      }
    }
  }
  return set;
}
;
HashSetImplementation$Dart.prototype.clear$member = function(){
  this._backingMap$$getter_().clear$named(0, $noargs);
}
;
HashSetImplementation$Dart.prototype.clear$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return HashSetImplementation$Dart.prototype.clear$member.call(this);
}
;
HashSetImplementation$Dart.prototype.clear$getter = function clear$getter(){
  return $bind(HashSetImplementation$Dart.prototype.clear$named, this);
}
;
HashSetImplementation$Dart.prototype.add$member = function(value){
  var tmp$0;
  this._backingMap$$getter_().ASSIGN_INDEX$operator(value, tmp$0 = value) , tmp$0;
}
;
HashSetImplementation$Dart.prototype.add$named = function($n, $o, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart.prototype.add$member.call(this, value);
}
;
HashSetImplementation$Dart.prototype.add$getter = function add$getter(){
  return $bind(HashSetImplementation$Dart.prototype.add$named, this);
}
;
HashSetImplementation$Dart.prototype.contains$member = function(value){
  return this._backingMap$$getter_().containsKey$named(1, $noargs, value);
}
;
HashSetImplementation$Dart.prototype.contains$named = function($n, $o, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart.prototype.contains$member.call(this, value);
}
;
HashSetImplementation$Dart.prototype.contains$getter = function contains$getter(){
  return $bind(HashSetImplementation$Dart.prototype.contains$named, this);
}
;
HashSetImplementation$Dart.prototype.remove$member = function(value){
  if (!this._backingMap$$getter_().containsKey$named(1, $noargs, value)) {
    return false;
  }
  this._backingMap$$getter_().remove$named(1, $noargs, value);
  return true;
}
;
HashSetImplementation$Dart.prototype.remove$named = function($n, $o, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart.prototype.remove$member.call(this, value);
}
;
HashSetImplementation$Dart.prototype.remove$getter = function remove$getter(){
  return $bind(HashSetImplementation$Dart.prototype.remove$named, this);
}
;
function HashSetImplementation$Dart$addAll$c0$_$26_6_2$Hoisted(value){
  this.add$member(value);
}

function HashSetImplementation$Dart$addAll$c0$_$26_6_2$Hoisted$named($n, $o, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart$addAll$c0$_$26_6_2$Hoisted.call(this, value);
}

HashSetImplementation$Dart.prototype.addAll$member = function(collection){
  collection.forEach$named(1, $noargs, $bind(HashSetImplementation$Dart$addAll$c0$_$26_6_2$Hoisted$named, this));
}
;
HashSetImplementation$Dart.prototype.addAll$named = function($n, $o, collection){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart.prototype.addAll$member.call(this, collection);
}
;
HashSetImplementation$Dart.prototype.addAll$getter = function addAll$getter(){
  return $bind(HashSetImplementation$Dart.prototype.addAll$named, this);
}
;
function HashSetImplementation$Dart$intersection$c0$_$26_12_2$Hoisted(dartc_scp$1, value){
  if (this.contains$member(value)) {
    dartc_scp$1.result.add$named(1, $noargs, value);
  }
}

function HashSetImplementation$Dart$intersection$c0$_$26_12_2$Hoisted$named($s0, $n, $o, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart$intersection$c0$_$26_12_2$Hoisted.call(this, $s0, value);
}

HashSetImplementation$Dart.prototype.intersection$member = function(collection){
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.result = HashSetImplementation$Dart.HashSetImplementation$$Factory(HashSetImplementation$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('HashSetImplementation$Dart')), 0)]));
  collection.forEach$named(1, $noargs, $bind(HashSetImplementation$Dart$intersection$c0$_$26_12_2$Hoisted$named, this, dartc_scp$1));
  return dartc_scp$1.result;
  dartc_scp$1 = $Dart$Null;
}
;
HashSetImplementation$Dart.prototype.intersection$named = function($n, $o, collection){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart.prototype.intersection$member.call(this, collection);
}
;
HashSetImplementation$Dart.prototype.intersection$getter = function intersection$getter(){
  return $bind(HashSetImplementation$Dart.prototype.intersection$named, this);
}
;
HashSetImplementation$Dart.prototype.isSubsetOf$member = function(other){
  return HashSetImplementation$Dart.HashSetImplementation$from$21$Factory(other).containsAll$named(1, $noargs, this);
}
;
HashSetImplementation$Dart.prototype.isSubsetOf$named = function($n, $o, other){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart.prototype.isSubsetOf$member.call(this, other);
}
;
HashSetImplementation$Dart.prototype.isSubsetOf$getter = function isSubsetOf$getter(){
  return $bind(HashSetImplementation$Dart.prototype.isSubsetOf$named, this);
}
;
function HashSetImplementation$Dart$removeAll$c0$_$26_9_2$Hoisted(value){
  this.remove$member(value);
}

function HashSetImplementation$Dart$removeAll$c0$_$26_9_2$Hoisted$named($n, $o, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart$removeAll$c0$_$26_9_2$Hoisted.call(this, value);
}

HashSetImplementation$Dart.prototype.removeAll$member = function(collection){
  collection.forEach$named(1, $noargs, $bind(HashSetImplementation$Dart$removeAll$c0$_$26_9_2$Hoisted$named, this));
}
;
HashSetImplementation$Dart.prototype.removeAll$named = function($n, $o, collection){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart.prototype.removeAll$member.call(this, collection);
}
;
HashSetImplementation$Dart.prototype.removeAll$getter = function removeAll$getter(){
  return $bind(HashSetImplementation$Dart.prototype.removeAll$named, this);
}
;
function HashSetImplementation$Dart$containsAll$c0$_$26_11_2$Hoisted(value){
  return this.contains$member(value);
}

function HashSetImplementation$Dart$containsAll$c0$_$26_11_2$Hoisted$named($n, $o, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart$containsAll$c0$_$26_11_2$Hoisted.call(this, value);
}

HashSetImplementation$Dart.prototype.containsAll$member = function(collection){
  return collection.every$named(1, $noargs, $bind(HashSetImplementation$Dart$containsAll$c0$_$26_11_2$Hoisted$named, this));
}
;
HashSetImplementation$Dart.prototype.containsAll$named = function($n, $o, collection){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart.prototype.containsAll$member.call(this, collection);
}
;
HashSetImplementation$Dart.prototype.containsAll$getter = function containsAll$getter(){
  return $bind(HashSetImplementation$Dart.prototype.containsAll$named, this);
}
;
function HashSetImplementation$Dart$forEach$c0$_$26_7_2$Hoisted(dartc_scp$0, key, value){
  dartc_scp$0.f(1, $noargs, key);
}

function HashSetImplementation$Dart$forEach$c0$_$26_7_2$Hoisted$named($s0, $n, $o, key, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return HashSetImplementation$Dart$forEach$c0$_$26_7_2$Hoisted($s0, key, value);
}

HashSetImplementation$Dart.prototype.forEach$member = function(f){
  var dartc_scp$0 = {f:f};
  this._backingMap$$getter_().forEach$named(1, $noargs, $bind(HashSetImplementation$Dart$forEach$c0$_$26_7_2$Hoisted$named, $Dart$Null, dartc_scp$0));
}
;
HashSetImplementation$Dart.prototype.forEach$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart.prototype.forEach$member.call(this, f);
}
;
HashSetImplementation$Dart.prototype.forEach$getter = function forEach$getter(){
  return $bind(HashSetImplementation$Dart.prototype.forEach$named, this);
}
;
function HashSetImplementation$Dart$filter$c0$_$26_6_2$Hoisted(dartc_scp$0, dartc_scp$1, key, value){
  if (dartc_scp$0.f(1, $noargs, key)) {
    dartc_scp$1.result.add$named(1, $noargs, key);
  }
}

function HashSetImplementation$Dart$filter$c0$_$26_6_2$Hoisted$named($s0, $s1, $n, $o, key, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return HashSetImplementation$Dart$filter$c0$_$26_6_2$Hoisted($s0, $s1, key, value);
}

HashSetImplementation$Dart.prototype.filter$member = function(f){
  var dartc_scp$0 = {f:f};
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.result = HashSetImplementation$Dart.HashSetImplementation$$Factory(HashSetImplementation$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('HashSetImplementation$Dart')), 0)]));
  this._backingMap$$getter_().forEach$named(1, $noargs, $bind(HashSetImplementation$Dart$filter$c0$_$26_6_2$Hoisted$named, $Dart$Null, dartc_scp$0, dartc_scp$1));
  return dartc_scp$1.result;
  dartc_scp$1 = $Dart$Null;
}
;
HashSetImplementation$Dart.prototype.filter$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart.prototype.filter$member.call(this, f);
}
;
HashSetImplementation$Dart.prototype.filter$getter = function filter$getter(){
  return $bind(HashSetImplementation$Dart.prototype.filter$named, this);
}
;
HashSetImplementation$Dart.prototype.every$member = function(f){
  var keys = this._backingMap$$getter_().getKeys$named(0, $noargs);
  return keys.every$named(1, $noargs, f);
}
;
HashSetImplementation$Dart.prototype.every$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart.prototype.every$member.call(this, f);
}
;
HashSetImplementation$Dart.prototype.every$getter = function every$getter(){
  return $bind(HashSetImplementation$Dart.prototype.every$named, this);
}
;
HashSetImplementation$Dart.prototype.some$member = function(f){
  var keys = this._backingMap$$getter_().getKeys$named(0, $noargs);
  return keys.some$named(1, $noargs, f);
}
;
HashSetImplementation$Dart.prototype.some$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return HashSetImplementation$Dart.prototype.some$member.call(this, f);
}
;
HashSetImplementation$Dart.prototype.some$getter = function some$getter(){
  return $bind(HashSetImplementation$Dart.prototype.some$named, this);
}
;
HashSetImplementation$Dart.prototype.isEmpty$member = function(){
  return this._backingMap$$getter_().isEmpty$named(0, $noargs);
}
;
HashSetImplementation$Dart.prototype.isEmpty$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return HashSetImplementation$Dart.prototype.isEmpty$member.call(this);
}
;
HashSetImplementation$Dart.prototype.isEmpty$getter = function isEmpty$getter(){
  return $bind(HashSetImplementation$Dart.prototype.isEmpty$named, this);
}
;
HashSetImplementation$Dart.prototype.length$named = function(){
  return this.length$getter().apply(this, arguments);
}
;
HashSetImplementation$Dart.prototype.length$getter = function(){
  return this._backingMap$$getter_().length$getter();
}
;
HashSetImplementation$Dart.prototype.iterator$member = function(){
  return HashSetIterator$Dart.HashSetIterator$$Factory(HashSetIterator$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('HashSetImplementation$Dart')), 0)]), this);
}
;
HashSetImplementation$Dart.prototype.iterator$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return HashSetImplementation$Dart.prototype.iterator$member.call(this);
}
;
HashSetImplementation$Dart.prototype.iterator$getter = function iterator$getter(){
  return $bind(HashSetImplementation$Dart.prototype.iterator$named, this);
}
;
HashSetImplementation$Dart.prototype._backingMap$$named_ = function(){
  return this._backingMap$$getter_().apply(this, arguments);
}
;
HashSetImplementation$Dart.prototype._backingMap$$getter_ = function(){
  return this._backingMap$$field_;
}
;
HashSetImplementation$Dart.prototype._backingMap$$setter_ = function(tmp$0){
  this._backingMap$$field_ = tmp$0;
}
;
function HashSetIterator$Dart(){
}

HashSetIterator$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('HashSetIterator$Dart'), HashSetIterator$Dart.$RTTimplements, typeArgs);
}
;
HashSetIterator$Dart.$RTTimplements = function(rtt, typeArgs){
  HashSetIterator$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
HashSetIterator$Dart.$addTo = function(target, typeArgs){
  var rtt = HashSetIterator$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  Iterator$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0)]);
}
;
HashSetIterator$Dart.prototype.$implements$HashSetIterator$Dart = 1;
HashSetIterator$Dart.prototype.$implements$Iterator$Dart = 1;
HashSetIterator$Dart.prototype.$implements$Object$Dart = 1;
HashSetIterator$Dart.$Constructor = function(set_){
  Object.$Constructor.call(this);
  this._advance$$member_();
}
;
HashSetIterator$Dart.$Initializer = function(set_){
  Object.$Initializer.call(this);
  this._nextValidIndex$$field_ = negate$operator(1);
  this._entries$$field_ = set_._backingMap$$getter_()._keys$$getter_();
}
;
HashSetIterator$Dart.HashSetIterator$$Factory = function($rtt, set_){
  var tmp$0 = new HashSetIterator$Dart;
  tmp$0.$typeInfo = $rtt;
  HashSetIterator$Dart.$Initializer.call(tmp$0, set_);
  HashSetIterator$Dart.$Constructor.call(tmp$0, set_);
  return tmp$0;
}
;
HashSetIterator$Dart.prototype.hasNext$member = function(){
  if (GTE$operator(this._nextValidIndex$$getter_(), this._entries$$getter_().length$getter())) {
    return false;
  }
  if (this._entries$$getter_().INDEX$operator(this._nextValidIndex$$getter_()) === HashMapImplementation$Dart._deletedKey$$getter_()) {
    this._advance$$member_();
  }
  return LT$operator(this._nextValidIndex$$getter_(), this._entries$$getter_().length$getter());
}
;
HashSetIterator$Dart.prototype.hasNext$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return HashSetIterator$Dart.prototype.hasNext$member.call(this);
}
;
HashSetIterator$Dart.prototype.hasNext$getter = function hasNext$getter(){
  return $bind(HashSetIterator$Dart.prototype.hasNext$named, this);
}
;
HashSetIterator$Dart.prototype.next$member = function(){
  if (!this.hasNext$member()) {
    $Dart$ThrowException($intern(NoMoreElementsException$Dart.NoMoreElementsException$$Factory()));
  }
  var res = this._entries$$getter_().INDEX$operator(this._nextValidIndex$$getter_());
  this._advance$$member_();
  return res;
}
;
HashSetIterator$Dart.prototype.next$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return HashSetIterator$Dart.prototype.next$member.call(this);
}
;
HashSetIterator$Dart.prototype.next$getter = function next$getter(){
  return $bind(HashSetIterator$Dart.prototype.next$named, this);
}
;
HashSetIterator$Dart.prototype._advance$$member_ = function(){
  var tmp$0;
  var length_0 = this._entries$$getter_().length$getter();
  var entry = $Dart$Null;
  var deletedKey = HashMapImplementation$Dart._deletedKey$$getter_();
  do {
    if (GTE$operator((this._nextValidIndex$$setter_(tmp$0 = ADD$operator(this._nextValidIndex$$getter_(), 1)) , tmp$0), length_0)) {
      break;
    }
    entry = this._entries$$getter_().INDEX$operator(this._nextValidIndex$$getter_());
  }
   while (entry == null || entry === deletedKey);
}
;
HashSetIterator$Dart.prototype._advance$$named_ = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return HashSetIterator$Dart.prototype._advance$$member_.call(this);
}
;
HashSetIterator$Dart.prototype._advance$$getter_ = function _advance$$getter_(){
  return $bind(HashSetIterator$Dart.prototype._advance$$named_, this);
}
;
HashSetIterator$Dart.prototype._entries$$named_ = function(){
  return this._entries$$getter_().apply(this, arguments);
}
;
HashSetIterator$Dart.prototype._entries$$getter_ = function(){
  return this._entries$$field_;
}
;
HashSetIterator$Dart.prototype._entries$$setter_ = function(tmp$0){
  this._entries$$field_ = tmp$0;
}
;
HashSetIterator$Dart.prototype._nextValidIndex$$named_ = function(){
  return this._nextValidIndex$$getter_().apply(this, arguments);
}
;
HashSetIterator$Dart.prototype._nextValidIndex$$getter_ = function(){
  return this._nextValidIndex$$field_;
}
;
HashSetIterator$Dart.prototype._nextValidIndex$$setter_ = function(tmp$0){
  this._nextValidIndex$$field_ = tmp$0;
}
;
function KeyValuePair$Dart(){
}

KeyValuePair$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('KeyValuePair$Dart'), null, typeArgs);
}
;
KeyValuePair$Dart.$addTo = function(target, typeArgs){
  var rtt = KeyValuePair$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
}
;
KeyValuePair$Dart.prototype.$implements$KeyValuePair$Dart = 1;
KeyValuePair$Dart.prototype.$implements$Object$Dart = 1;
KeyValuePair$Dart.$Constructor = function(key, value){
  Object.$Constructor.call(this);
}
;
KeyValuePair$Dart.$Initializer = function(key, value){
  Object.$Initializer.call(this);
  this.key$field = key;
  this.value$field = value;
}
;
KeyValuePair$Dart.KeyValuePair$$Factory = function($rtt, key, value){
  var tmp$0 = new KeyValuePair$Dart;
  tmp$0.$typeInfo = $rtt;
  KeyValuePair$Dart.$Initializer.call(tmp$0, key, value);
  KeyValuePair$Dart.$Constructor.call(tmp$0, key, value);
  return tmp$0;
}
;
KeyValuePair$Dart.prototype.key$named = function(){
  return this.key$getter().apply(this, arguments);
}
;
KeyValuePair$Dart.prototype.key$getter = function(){
  return this.key$field;
}
;
KeyValuePair$Dart.prototype.value$named = function(){
  return this.value$getter().apply(this, arguments);
}
;
KeyValuePair$Dart.prototype.value$getter = function(){
  return this.value$field;
}
;
KeyValuePair$Dart.prototype.value$setter = function(tmp$0){
  this.value$field = tmp$0;
}
;
function LinkedHashMapImplementation$Dart(){
}

LinkedHashMapImplementation$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('LinkedHashMapImplementation$Dart'), LinkedHashMapImplementation$Dart.$RTTimplements, typeArgs);
}
;
LinkedHashMapImplementation$Dart.$RTTimplements = function(rtt, typeArgs){
  LinkedHashMapImplementation$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
LinkedHashMapImplementation$Dart.$addTo = function(target, typeArgs){
  var rtt = LinkedHashMapImplementation$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  LinkedHashMap$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0), RTT.getTypeArg(target.typeArgs, 1)]);
}
;
LinkedHashMapImplementation$Dart.prototype.$implements$LinkedHashMapImplementation$Dart = 1;
LinkedHashMapImplementation$Dart.prototype.$implements$LinkedHashMap$Dart = 1;
LinkedHashMapImplementation$Dart.prototype.$implements$HashMap$Dart = 1;
LinkedHashMapImplementation$Dart.prototype.$implements$Map$Dart = 1;
LinkedHashMapImplementation$Dart.prototype.$implements$Object$Dart = 1;
LinkedHashMapImplementation$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
  var tmp$1, tmp$0;
  this._map$$setter_(tmp$0 = HashMapImplementation$Dart.HashMapImplementation$$Factory(HashMapImplementation$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('LinkedHashMapImplementation$Dart')), 0), DoubleLinkedQueueEntry$Dart.$lookupRTT([KeyValuePair$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('LinkedHashMapImplementation$Dart')), 0), RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('LinkedHashMapImplementation$Dart')), 1)])])]))) , tmp$0;
  this._list$$setter_(tmp$1 = DoubleLinkedQueue$Dart.DoubleLinkedQueue$$Factory(DoubleLinkedQueue$Dart.$lookupRTT([KeyValuePair$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('LinkedHashMapImplementation$Dart')), 0), RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('LinkedHashMapImplementation$Dart')), 1)])]))) , tmp$1;
}
;
LinkedHashMapImplementation$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
LinkedHashMapImplementation$Dart.LinkedHashMapImplementation$$Factory = function($rtt){
  var tmp$0 = new LinkedHashMapImplementation$Dart;
  tmp$0.$typeInfo = $rtt;
  LinkedHashMapImplementation$Dart.$Initializer.call(tmp$0);
  LinkedHashMapImplementation$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function LinkedHashMapImplementation$Dart$from$c0$32_32$HoistedConstructor(dartc_scp$1, key, value){
  var tmp$0;
  dartc_scp$1.result.ASSIGN_INDEX$operator(key, tmp$0 = value) , tmp$0;
}

function LinkedHashMapImplementation$Dart$from$c0$32_32$HoistedConstructor$named($s0, $n, $o, key, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return LinkedHashMapImplementation$Dart$from$c0$32_32$HoistedConstructor($s0, key, value);
}

LinkedHashMapImplementation$Dart.LinkedHashMapImplementation$from$27$Factory = function(other){
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.result = LinkedHashMapImplementation$Dart.LinkedHashMapImplementation$$Factory(LinkedHashMapImplementation$Dart.$lookupRTT());
  other.forEach$named(1, $noargs, $bind(LinkedHashMapImplementation$Dart$from$c0$32_32$HoistedConstructor$named, $Dart$Null, dartc_scp$1));
  return dartc_scp$1.result;
  dartc_scp$1 = $Dart$Null;
}
;
LinkedHashMapImplementation$Dart.prototype._list$$named_ = function(){
  return this._list$$getter_().apply(this, arguments);
}
;
LinkedHashMapImplementation$Dart.prototype._list$$getter_ = function(){
  return this._list$$field_;
}
;
LinkedHashMapImplementation$Dart.prototype._list$$setter_ = function(tmp$0){
  this._list$$field_ = tmp$0;
}
;
LinkedHashMapImplementation$Dart.prototype._map$$named_ = function(){
  return this._map$$getter_().apply(this, arguments);
}
;
LinkedHashMapImplementation$Dart.prototype._map$$getter_ = function(){
  return this._map$$field_;
}
;
LinkedHashMapImplementation$Dart.prototype._map$$setter_ = function(tmp$0){
  this._map$$field_ = tmp$0;
}
;
LinkedHashMapImplementation$Dart.prototype.ASSIGN_INDEX$operator = function(key, value){
  var tmp$1, tmp$0;
  if (this._map$$getter_().containsKey$named(1, $noargs, key)) {
    this._map$$getter_().INDEX$operator(key).element$getter().value$setter(tmp$0 = value) , tmp$0;
  }
   else {
    this._list$$getter_().addLast$named(1, $noargs, KeyValuePair$Dart.KeyValuePair$$Factory(KeyValuePair$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('LinkedHashMapImplementation$Dart')), 0), RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('LinkedHashMapImplementation$Dart')), 1)]), key, value));
    this._map$$getter_().ASSIGN_INDEX$operator(key, tmp$1 = this._list$$getter_().lastEntry$named(0, $noargs)) , tmp$1;
  }
}
;
LinkedHashMapImplementation$Dart.prototype.INDEX$operator = function(key){
  var entry = this._map$$getter_().INDEX$operator(key);
  if (entry == null) {
    return $Dart$Null;
  }
  return entry.element$getter().value$getter();
}
;
LinkedHashMapImplementation$Dart.prototype.remove$member = function(key){
  var entry = this._map$$getter_().remove$named(1, $noargs, key);
  if (entry == null) {
    return $Dart$Null;
  }
  entry.remove$named(0, $noargs);
  return entry.element$getter().value$getter();
}
;
LinkedHashMapImplementation$Dart.prototype.remove$named = function($n, $o, key){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return LinkedHashMapImplementation$Dart.prototype.remove$member.call(this, key);
}
;
LinkedHashMapImplementation$Dart.prototype.remove$getter = function remove$getter(){
  return $bind(LinkedHashMapImplementation$Dart.prototype.remove$named, this);
}
;
LinkedHashMapImplementation$Dart.prototype.putIfAbsent$member = function(key, ifAbsent){
  var tmp$0;
  var value = this.INDEX$operator(key);
  if (this.INDEX$operator(key) == null && !this.containsKey$member(key)) {
    value = ifAbsent(0, $noargs);
    this.ASSIGN_INDEX$operator(key, tmp$0 = value) , tmp$0;
  }
  return value;
}
;
LinkedHashMapImplementation$Dart.prototype.putIfAbsent$named = function($n, $o, key, ifAbsent){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return LinkedHashMapImplementation$Dart.prototype.putIfAbsent$member.call(this, key, ifAbsent);
}
;
LinkedHashMapImplementation$Dart.prototype.putIfAbsent$getter = function putIfAbsent$getter(){
  return $bind(LinkedHashMapImplementation$Dart.prototype.putIfAbsent$named, this);
}
;
function LinkedHashMapImplementation$Dart$getKeys$c0$_$32_7_2$Hoisted(dartc_scp$1, entry){
  var tmp$1, tmp$0;
  dartc_scp$1.list.ASSIGN_INDEX$operator((tmp$0 = dartc_scp$1.index , (dartc_scp$1.index = ADD$operator(tmp$0, 1) , tmp$0)), tmp$1 = entry.key$getter()) , tmp$1;
}

function LinkedHashMapImplementation$Dart$getKeys$c0$_$32_7_2$Hoisted$named($s0, $n, $o, entry){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return LinkedHashMapImplementation$Dart$getKeys$c0$_$32_7_2$Hoisted($s0, entry);
}

LinkedHashMapImplementation$Dart.prototype.getKeys$member = function(){
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.list = ListFactory$Dart.List$$Factory([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('LinkedHashMapImplementation$Dart')), 0)], this.length$getter());
  dartc_scp$1.index = 0;
  this._list$$getter_().forEach$named(1, $noargs, $bind(LinkedHashMapImplementation$Dart$getKeys$c0$_$32_7_2$Hoisted$named, $Dart$Null, dartc_scp$1));
  assert(EQ$operator(dartc_scp$1.index, this.length$getter()));
  return dartc_scp$1.list;
  dartc_scp$1 = $Dart$Null;
}
;
LinkedHashMapImplementation$Dart.prototype.getKeys$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return LinkedHashMapImplementation$Dart.prototype.getKeys$member.call(this);
}
;
LinkedHashMapImplementation$Dart.prototype.getKeys$getter = function getKeys$getter(){
  return $bind(LinkedHashMapImplementation$Dart.prototype.getKeys$named, this);
}
;
function LinkedHashMapImplementation$Dart$getValues$c0$_$32_9_2$Hoisted(dartc_scp$1, entry){
  var tmp$1, tmp$0;
  dartc_scp$1.list.ASSIGN_INDEX$operator((tmp$0 = dartc_scp$1.index , (dartc_scp$1.index = ADD$operator(tmp$0, 1) , tmp$0)), tmp$1 = entry.value$getter()) , tmp$1;
}

function LinkedHashMapImplementation$Dart$getValues$c0$_$32_9_2$Hoisted$named($s0, $n, $o, entry){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return LinkedHashMapImplementation$Dart$getValues$c0$_$32_9_2$Hoisted($s0, entry);
}

LinkedHashMapImplementation$Dart.prototype.getValues$member = function(){
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.list = ListFactory$Dart.List$$Factory([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('LinkedHashMapImplementation$Dart')), 1)], this.length$getter());
  dartc_scp$1.index = 0;
  this._list$$getter_().forEach$named(1, $noargs, $bind(LinkedHashMapImplementation$Dart$getValues$c0$_$32_9_2$Hoisted$named, $Dart$Null, dartc_scp$1));
  assert(EQ$operator(dartc_scp$1.index, this.length$getter()));
  return dartc_scp$1.list;
  dartc_scp$1 = $Dart$Null;
}
;
LinkedHashMapImplementation$Dart.prototype.getValues$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return LinkedHashMapImplementation$Dart.prototype.getValues$member.call(this);
}
;
LinkedHashMapImplementation$Dart.prototype.getValues$getter = function getValues$getter(){
  return $bind(LinkedHashMapImplementation$Dart.prototype.getValues$named, this);
}
;
function LinkedHashMapImplementation$Dart$forEach$c0$_$32_7_2$Hoisted(dartc_scp$0, entry){
  dartc_scp$0.f(2, $noargs, entry.key$getter(), entry.value$getter());
}

function LinkedHashMapImplementation$Dart$forEach$c0$_$32_7_2$Hoisted$named($s0, $n, $o, entry){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return LinkedHashMapImplementation$Dart$forEach$c0$_$32_7_2$Hoisted($s0, entry);
}

LinkedHashMapImplementation$Dart.prototype.forEach$member = function(f){
  var dartc_scp$0 = {f:f};
  this._list$$getter_().forEach$named(1, $noargs, $bind(LinkedHashMapImplementation$Dart$forEach$c0$_$32_7_2$Hoisted$named, $Dart$Null, dartc_scp$0));
}
;
LinkedHashMapImplementation$Dart.prototype.forEach$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return LinkedHashMapImplementation$Dart.prototype.forEach$member.call(this, f);
}
;
LinkedHashMapImplementation$Dart.prototype.forEach$getter = function forEach$getter(){
  return $bind(LinkedHashMapImplementation$Dart.prototype.forEach$named, this);
}
;
LinkedHashMapImplementation$Dart.prototype.containsKey$member = function(key){
  return this._map$$getter_().containsKey$named(1, $noargs, key);
}
;
LinkedHashMapImplementation$Dart.prototype.containsKey$named = function($n, $o, key){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return LinkedHashMapImplementation$Dart.prototype.containsKey$member.call(this, key);
}
;
LinkedHashMapImplementation$Dart.prototype.containsKey$getter = function containsKey$getter(){
  return $bind(LinkedHashMapImplementation$Dart.prototype.containsKey$named, this);
}
;
function LinkedHashMapImplementation$Dart$containsValue$c0$_$32_13_2$Hoisted(dartc_scp$0, entry){
  return EQ$operator(entry.value$getter(), dartc_scp$0.value);
}

function LinkedHashMapImplementation$Dart$containsValue$c0$_$32_13_2$Hoisted$named($s0, $n, $o, entry){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return LinkedHashMapImplementation$Dart$containsValue$c0$_$32_13_2$Hoisted($s0, entry);
}

LinkedHashMapImplementation$Dart.prototype.containsValue$member = function(value){
  var dartc_scp$0 = {value:value};
  return this._list$$getter_().some$named(1, $noargs, $bind(LinkedHashMapImplementation$Dart$containsValue$c0$_$32_13_2$Hoisted$named, $Dart$Null, dartc_scp$0));
}
;
LinkedHashMapImplementation$Dart.prototype.containsValue$named = function($n, $o, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return LinkedHashMapImplementation$Dart.prototype.containsValue$member.call(this, value);
}
;
LinkedHashMapImplementation$Dart.prototype.containsValue$getter = function containsValue$getter(){
  return $bind(LinkedHashMapImplementation$Dart.prototype.containsValue$named, this);
}
;
LinkedHashMapImplementation$Dart.prototype.length$named = function(){
  return this.length$getter().apply(this, arguments);
}
;
LinkedHashMapImplementation$Dart.prototype.length$getter = function(){
  return this._map$$getter_().length$getter();
}
;
LinkedHashMapImplementation$Dart.prototype.isEmpty$member = function(){
  return EQ$operator(this.length$getter(), 0);
}
;
LinkedHashMapImplementation$Dart.prototype.isEmpty$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return LinkedHashMapImplementation$Dart.prototype.isEmpty$member.call(this);
}
;
LinkedHashMapImplementation$Dart.prototype.isEmpty$getter = function isEmpty$getter(){
  return $bind(LinkedHashMapImplementation$Dart.prototype.isEmpty$named, this);
}
;
LinkedHashMapImplementation$Dart.prototype.clear$member = function(){
  this._map$$getter_().clear$named(0, $noargs);
  this._list$$getter_().clear$named(0, $noargs);
}
;
LinkedHashMapImplementation$Dart.prototype.clear$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return LinkedHashMapImplementation$Dart.prototype.clear$member.call(this);
}
;
LinkedHashMapImplementation$Dart.prototype.clear$getter = function clear$getter(){
  return $bind(LinkedHashMapImplementation$Dart.prototype.clear$named, this);
}
;
function Maps$Dart(){
}

Maps$Dart.$lookupRTT = function(){
  return RTT.create($cls('Maps$Dart'));
}
;
Maps$Dart.$addTo = function(target){
  var rtt = Maps$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
Maps$Dart.prototype.$implements$Maps$Dart = 1;
Maps$Dart.prototype.$implements$Object$Dart = 1;
Maps$Dart.containsValue$member = function(map, value){
  {
    var $0 = map.getValues$named(0, $noargs).iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var v = $0.next$named(0, $noargs);
      {
        if (EQ$operator(value, v)) {
          return true;
        }
      }
    }
  }
  return false;
}
;
Maps$Dart.containsValue$named = function($n, $o, map, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Maps$Dart.containsValue$member(map, value);
}
;
Maps$Dart.containsValue$getter = function containsValue$getter(){
  return Maps$Dart.containsValue$named;
}
;
Maps$Dart.containsKey$member = function(map, key){
  {
    var $0 = map.getKeys$named(0, $noargs).iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var k = $0.next$named(0, $noargs);
      {
        if (EQ$operator(key, k)) {
          return true;
        }
      }
    }
  }
  return false;
}
;
Maps$Dart.containsKey$named = function($n, $o, map, key){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Maps$Dart.containsKey$member(map, key);
}
;
Maps$Dart.containsKey$getter = function containsKey$getter(){
  return Maps$Dart.containsKey$named;
}
;
Maps$Dart.putIfAbsent$member = function(map, key, ifAbsent){
  var tmp$0;
  if (map.containsKey$named(1, $noargs, key)) {
    return map.INDEX$operator(key);
  }
  var v = ifAbsent(0, $noargs);
  map.ASSIGN_INDEX$operator(key, tmp$0 = v) , tmp$0;
  return v;
}
;
Maps$Dart.putIfAbsent$named = function($n, $o, map, key, ifAbsent){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return Maps$Dart.putIfAbsent$member(map, key, ifAbsent);
}
;
Maps$Dart.putIfAbsent$getter = function putIfAbsent$getter(){
  return Maps$Dart.putIfAbsent$named;
}
;
Maps$Dart.clear$member = function(map){
  {
    var $0 = map.getKeys$named(0, $noargs).iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var k = $0.next$named(0, $noargs);
      {
        map.remove$named(1, $noargs, k);
      }
    }
  }
}
;
Maps$Dart.clear$named = function($n, $o, map){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Maps$Dart.clear$member(map);
}
;
Maps$Dart.clear$getter = function clear$getter(){
  return Maps$Dart.clear$named;
}
;
Maps$Dart.forEach$member = function(map, f){
  {
    var $0 = map.getKeys$named(0, $noargs).iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var k = $0.next$named(0, $noargs);
      {
        f(2, $noargs, k, map.INDEX$operator(k));
      }
    }
  }
}
;
Maps$Dart.forEach$named = function($n, $o, map, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Maps$Dart.forEach$member(map, f);
}
;
Maps$Dart.forEach$getter = function forEach$getter(){
  return Maps$Dart.forEach$named;
}
;
Maps$Dart.getValues$member = function(map){
  var result = RTT.setTypeInfo([], Array.$lookupRTT());
  {
    var $0 = map.getKeys$named(0, $noargs).iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var k = $0.next$named(0, $noargs);
      {
        result.add$named(1, $noargs, map.INDEX$operator(k));
      }
    }
  }
  return result;
}
;
Maps$Dart.getValues$named = function($n, $o, map){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Maps$Dart.getValues$member(map);
}
;
Maps$Dart.getValues$getter = function getValues$getter(){
  return Maps$Dart.getValues$named;
}
;
Maps$Dart.length$member = function(map){
  return map.getKeys$named(0, $noargs).length$getter();
}
;
Maps$Dart.length$named = function($n, $o, map){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Maps$Dart.length$member(map);
}
;
Maps$Dart.length$getter = function length$getter(){
  return Maps$Dart.length$named;
}
;
Maps$Dart.isEmpty$member = function(map){
  return EQ$operator(Maps$Dart.length$member(map), 0);
}
;
Maps$Dart.isEmpty$named = function($n, $o, map){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Maps$Dart.isEmpty$member(map);
}
;
Maps$Dart.isEmpty$getter = function isEmpty$getter(){
  return Maps$Dart.isEmpty$named;
}
;
Maps$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
Maps$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
Maps$Dart.Maps$$Factory = function(){
  var tmp$0 = new Maps$Dart;
  tmp$0.$typeInfo = Maps$Dart.$lookupRTT();
  Maps$Dart.$Initializer.call(tmp$0);
  Maps$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function PromiseImpl$Dart(){
}

PromiseImpl$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('PromiseImpl$Dart'), PromiseImpl$Dart.$RTTimplements, typeArgs);
}
;
PromiseImpl$Dart.$RTTimplements = function(rtt, typeArgs){
  PromiseImpl$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
PromiseImpl$Dart.$addTo = function(target, typeArgs){
  var rtt = PromiseImpl$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  Promise$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0)]);
}
;
PromiseImpl$Dart.prototype.$implements$PromiseImpl$Dart = 1;
PromiseImpl$Dart.prototype.$implements$Promise$Dart = 1;
PromiseImpl$Dart.prototype.$implements$Object$Dart = 1;
PromiseImpl$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
PromiseImpl$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
  this._state$$field_ = PromiseImpl$Dart.CREATED$getter();
  this._value$$field_ = $Dart$Null;
  this._error$$field_ = $Dart$Null;
  this._normalListeners$$field_ = $Dart$Null;
  this._errorListeners$$field_ = $Dart$Null;
  this._cancelListeners$$field_ = $Dart$Null;
}
;
PromiseImpl$Dart.PromiseImpl$$Factory = function($rtt){
  var tmp$0 = new PromiseImpl$Dart;
  tmp$0.$typeInfo = $rtt;
  PromiseImpl$Dart.$Initializer.call(tmp$0);
  PromiseImpl$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
PromiseImpl$Dart.fromValue$Constructor = function(val){
  Object.$Constructor.call(this);
}
;
PromiseImpl$Dart.fromValue$Initializer = function(val){
  Object.$Initializer.call(this);
  this._state$$field_ = PromiseImpl$Dart.COMPLETE_NORMAL$getter();
  this._value$$field_ = val;
  this._error$$field_ = $Dart$Null;
  this._normalListeners$$field_ = $Dart$Null;
  this._errorListeners$$field_ = $Dart$Null;
  this._cancelListeners$$field_ = $Dart$Null;
}
;
PromiseImpl$Dart.PromiseImpl$fromValue$11$Factory = function($rtt, val){
  var tmp$0 = new PromiseImpl$Dart;
  tmp$0.$typeInfo = $rtt;
  PromiseImpl$Dart.fromValue$Initializer.call(tmp$0, val);
  PromiseImpl$Dart.fromValue$Constructor.call(tmp$0, val);
  return tmp$0;
}
;
PromiseImpl$Dart.CREATED$named = function(){
  return PromiseImpl$Dart.CREATED$getter().apply(this, arguments);
}
;
PromiseImpl$Dart.CREATED$getter = function(){
  return 0;
}
;
PromiseImpl$Dart.RUNNING$named = function(){
  return PromiseImpl$Dart.RUNNING$getter().apply(this, arguments);
}
;
PromiseImpl$Dart.RUNNING$getter = function(){
  return 1;
}
;
PromiseImpl$Dart.COMPLETE_NORMAL$named = function(){
  return PromiseImpl$Dart.COMPLETE_NORMAL$getter().apply(this, arguments);
}
;
PromiseImpl$Dart.COMPLETE_NORMAL$getter = function(){
  return 2;
}
;
PromiseImpl$Dart.COMPLETE_ERROR$named = function(){
  return PromiseImpl$Dart.COMPLETE_ERROR$getter().apply(this, arguments);
}
;
PromiseImpl$Dart.COMPLETE_ERROR$getter = function(){
  return 3;
}
;
PromiseImpl$Dart.CANCELLED$named = function(){
  return PromiseImpl$Dart.CANCELLED$getter().apply(this, arguments);
}
;
PromiseImpl$Dart.CANCELLED$getter = function(){
  return 4;
}
;
PromiseImpl$Dart.COMPLETE_NORMAL_AFTER_CANCELLED$named = function(){
  return PromiseImpl$Dart.COMPLETE_NORMAL_AFTER_CANCELLED$getter().apply(this, arguments);
}
;
PromiseImpl$Dart.COMPLETE_NORMAL_AFTER_CANCELLED$getter = function(){
  return 5;
}
;
PromiseImpl$Dart.COMPLETE_ERROR_AFTER_CANCELLED$named = function(){
  return PromiseImpl$Dart.COMPLETE_ERROR_AFTER_CANCELLED$getter().apply(this, arguments);
}
;
PromiseImpl$Dart.COMPLETE_ERROR_AFTER_CANCELLED$getter = function(){
  return 6;
}
;
PromiseImpl$Dart.prototype._state$$named_ = function(){
  return this._state$$getter_().apply(this, arguments);
}
;
PromiseImpl$Dart.prototype._state$$getter_ = function(){
  return this._state$$field_;
}
;
PromiseImpl$Dart.prototype._state$$setter_ = function(tmp$0){
  this._state$$field_ = tmp$0;
}
;
PromiseImpl$Dart.prototype._value$$named_ = function(){
  return this._value$$getter_().apply(this, arguments);
}
;
PromiseImpl$Dart.prototype._value$$getter_ = function(){
  return this._value$$field_;
}
;
PromiseImpl$Dart.prototype._value$$setter_ = function(tmp$0){
  this._value$$field_ = tmp$0;
}
;
PromiseImpl$Dart.prototype._error$$named_ = function(){
  return this._error$$getter_().apply(this, arguments);
}
;
PromiseImpl$Dart.prototype._error$$getter_ = function(){
  return this._error$$field_;
}
;
PromiseImpl$Dart.prototype._error$$setter_ = function(tmp$0){
  this._error$$field_ = tmp$0;
}
;
PromiseImpl$Dart.prototype._normalListeners$$named_ = function(){
  return this._normalListeners$$getter_().apply(this, arguments);
}
;
PromiseImpl$Dart.prototype._normalListeners$$getter_ = function(){
  return this._normalListeners$$field_;
}
;
PromiseImpl$Dart.prototype._normalListeners$$setter_ = function(tmp$0){
  this._normalListeners$$field_ = tmp$0;
}
;
PromiseImpl$Dart.prototype._errorListeners$$named_ = function(){
  return this._errorListeners$$getter_().apply(this, arguments);
}
;
PromiseImpl$Dart.prototype._errorListeners$$getter_ = function(){
  return this._errorListeners$$field_;
}
;
PromiseImpl$Dart.prototype._errorListeners$$setter_ = function(tmp$0){
  this._errorListeners$$field_ = tmp$0;
}
;
PromiseImpl$Dart.prototype._cancelListeners$$named_ = function(){
  return this._cancelListeners$$getter_().apply(this, arguments);
}
;
PromiseImpl$Dart.prototype._cancelListeners$$getter_ = function(){
  return this._cancelListeners$$field_;
}
;
PromiseImpl$Dart.prototype._cancelListeners$$setter_ = function(tmp$0){
  this._cancelListeners$$field_ = tmp$0;
}
;
PromiseImpl$Dart.prototype.value$named = function(){
  return this.value$getter().apply(this, arguments);
}
;
PromiseImpl$Dart.prototype.value$getter = function(){
  if (!this.isDone$member()) {
    $Dart$ThrowException(ExceptionImplementation$Dart.ExceptionImplementation$$Factory('Attempted to get the value of an uncompleted promise.'));
  }
  if (this.hasError$member()) {
    $Dart$ThrowException(this._error$$getter_());
  }
   else {
    return this._value$$getter_();
  }
}
;
PromiseImpl$Dart.prototype.error$named = function(){
  return this.error$getter().apply(this, arguments);
}
;
PromiseImpl$Dart.prototype.error$getter = function(){
  if (!this.isDone$member()) {
    $Dart$ThrowException('Attempted to examine the state of an uncompleted promise.');
  }
  return this._error$$getter_();
}
;
PromiseImpl$Dart.prototype.isDone$member = function(){
  return NE$operator(this._state$$getter_(), PromiseImpl$Dart.CREATED$getter()) && NE$operator(this._state$$getter_(), PromiseImpl$Dart.RUNNING$getter());
}
;
PromiseImpl$Dart.prototype.isDone$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return PromiseImpl$Dart.prototype.isDone$member.call(this);
}
;
PromiseImpl$Dart.prototype.isDone$getter = function isDone$getter(){
  return $bind(PromiseImpl$Dart.prototype.isDone$named, this);
}
;
PromiseImpl$Dart.prototype.isCancelled$member = function(){
  return EQ$operator(this._state$$getter_(), PromiseImpl$Dart.CANCELLED$getter()) || EQ$operator(this._state$$getter_(), PromiseImpl$Dart.COMPLETE_NORMAL_AFTER_CANCELLED$getter()) || EQ$operator(this._state$$getter_(), PromiseImpl$Dart.COMPLETE_ERROR_AFTER_CANCELLED$getter());
}
;
PromiseImpl$Dart.prototype.isCancelled$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return PromiseImpl$Dart.prototype.isCancelled$member.call(this);
}
;
PromiseImpl$Dart.prototype.isCancelled$getter = function isCancelled$getter(){
  return $bind(PromiseImpl$Dart.prototype.isCancelled$named, this);
}
;
PromiseImpl$Dart.prototype.hasValue$member = function(){
  return EQ$operator(this._state$$getter_(), PromiseImpl$Dart.COMPLETE_NORMAL$getter()) || EQ$operator(this._state$$getter_(), PromiseImpl$Dart.COMPLETE_NORMAL_AFTER_CANCELLED$getter());
}
;
PromiseImpl$Dart.prototype.hasValue$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return PromiseImpl$Dart.prototype.hasValue$member.call(this);
}
;
PromiseImpl$Dart.prototype.hasValue$getter = function hasValue$getter(){
  return $bind(PromiseImpl$Dart.prototype.hasValue$named, this);
}
;
PromiseImpl$Dart.prototype.hasError$member = function(){
  return EQ$operator(this._state$$getter_(), PromiseImpl$Dart.COMPLETE_ERROR$getter()) || EQ$operator(this._state$$getter_(), PromiseImpl$Dart.COMPLETE_ERROR_AFTER_CANCELLED$getter());
}
;
PromiseImpl$Dart.prototype.hasError$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return PromiseImpl$Dart.prototype.hasError$member.call(this);
}
;
PromiseImpl$Dart.prototype.hasError$getter = function hasError$getter(){
  return $bind(PromiseImpl$Dart.prototype.hasError$named, this);
}
;
function PromiseImpl$Dart$complete$c0$16_16$Hoisted(dartc_scp$0, listener){
  listener(1, $noargs, dartc_scp$0.newVal);
}

function PromiseImpl$Dart$complete$c0$16_16$Hoisted$named($s0, $n, $o, listener){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart$complete$c0$16_16$Hoisted($s0, listener);
}

PromiseImpl$Dart.prototype.complete$member = function(newVal){
  var dartc_scp$0 = {newVal:newVal};
  var tmp$1, tmp$2, tmp$3, tmp$0;
  if (EQ$operator(this._state$$getter_(), PromiseImpl$Dart.CANCELLED$getter())) {
    this._value$$setter_(tmp$0 = dartc_scp$0.newVal) , tmp$0;
    this._state$$setter_(tmp$1 = PromiseImpl$Dart.COMPLETE_NORMAL_AFTER_CANCELLED$getter()) , tmp$1;
    return;
  }
  if (this.isDone$member()) {
    $Dart$ThrowException('Attempted to complete an already completed promise.');
  }
  this._value$$setter_(tmp$2 = dartc_scp$0.newVal) , tmp$2;
  this._state$$setter_(tmp$3 = PromiseImpl$Dart.COMPLETE_NORMAL$getter()) , tmp$3;
  if (this._normalListeners$$getter_() != null) {
    this._normalListeners$$getter_().forEach$named(1, $noargs, $bind(PromiseImpl$Dart$complete$c0$16_16$Hoisted$named, $Dart$Null, dartc_scp$0));
  }
  this._clearListeners$$member_();
}
;
PromiseImpl$Dart.prototype.complete$named = function($n, $o, newVal){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart.prototype.complete$member.call(this, newVal);
}
;
PromiseImpl$Dart.prototype.complete$getter = function complete$getter(){
  return $bind(PromiseImpl$Dart.prototype.complete$named, this);
}
;
PromiseImpl$Dart.prototype._clearListeners$$member_ = function(){
  var tmp$1, tmp$2, tmp$0;
  this._normalListeners$$setter_(tmp$0 = $Dart$Null) , tmp$0;
  this._errorListeners$$setter_(tmp$1 = $Dart$Null) , tmp$1;
  this._cancelListeners$$setter_(tmp$2 = $Dart$Null) , tmp$2;
}
;
PromiseImpl$Dart.prototype._clearListeners$$named_ = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return PromiseImpl$Dart.prototype._clearListeners$$member_.call(this);
}
;
PromiseImpl$Dart.prototype._clearListeners$$getter_ = function _clearListeners$$getter_(){
  return $bind(PromiseImpl$Dart.prototype._clearListeners$$named_, this);
}
;
function PromiseImpl$Dart$fail$c0$16_16$Hoisted(dartc_scp$0, listener){
  listener(1, $noargs, dartc_scp$0.err);
}

function PromiseImpl$Dart$fail$c0$16_16$Hoisted$named($s0, $n, $o, listener){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart$fail$c0$16_16$Hoisted($s0, listener);
}

PromiseImpl$Dart.prototype.fail$member = function(err){
  var dartc_scp$0 = {err:err};
  var tmp$1, tmp$2, tmp$3, tmp$0;
  if (EQ$operator(this._state$$getter_(), PromiseImpl$Dart.CANCELLED$getter())) {
    this._error$$setter_(tmp$0 = dartc_scp$0.err) , tmp$0;
    this._state$$setter_(tmp$1 = PromiseImpl$Dart.COMPLETE_ERROR_AFTER_CANCELLED$getter()) , tmp$1;
    return;
  }
  if (this.isDone$member()) {
    $Dart$ThrowException("Can't fail an already completed promise.");
  }
  this._error$$setter_(tmp$2 = dartc_scp$0.err) , tmp$2;
  this._state$$setter_(tmp$3 = PromiseImpl$Dart.COMPLETE_ERROR$getter()) , tmp$3;
  if (this._errorListeners$$getter_() != null) {
    this._errorListeners$$getter_().forEach$named(1, $noargs, $bind(PromiseImpl$Dart$fail$c0$16_16$Hoisted$named, $Dart$Null, dartc_scp$0));
  }
  this._clearListeners$$member_();
}
;
PromiseImpl$Dart.prototype.fail$named = function($n, $o, err){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart.prototype.fail$member.call(this, err);
}
;
PromiseImpl$Dart.prototype.fail$getter = function fail$getter(){
  return $bind(PromiseImpl$Dart.prototype.fail$named, this);
}
;
function PromiseImpl$Dart$cancel$c0$16_16$Hoisted(listener){
  listener(0, $noargs);
}

function PromiseImpl$Dart$cancel$c0$16_16$Hoisted$named($n, $o, listener){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart$cancel$c0$16_16$Hoisted(listener);
}

PromiseImpl$Dart.prototype.cancel$member = function(){
  var tmp$0;
  if (!this.isDone$member()) {
    this._state$$setter_(tmp$0 = PromiseImpl$Dart.CANCELLED$getter()) , tmp$0;
    if (this._cancelListeners$$getter_() != null) {
      this._cancelListeners$$getter_().forEach$named(1, $noargs, $bind(PromiseImpl$Dart$cancel$c0$16_16$Hoisted$named, $Dart$Null));
    }
    this._clearListeners$$member_();
    return true;
  }
  return false;
}
;
PromiseImpl$Dart.prototype.cancel$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return PromiseImpl$Dart.prototype.cancel$member.call(this);
}
;
PromiseImpl$Dart.prototype.cancel$getter = function cancel$getter(){
  return $bind(PromiseImpl$Dart.prototype.cancel$named, this);
}
;
PromiseImpl$Dart.prototype.addCompleteHandler$member = function(completeHandler){
  var tmp$0;
  if (EQ$operator(this._state$$getter_(), PromiseImpl$Dart.COMPLETE_NORMAL$getter())) {
    completeHandler(1, $noargs, this._value$$getter_());
  }
   else {
    if (!this.isDone$member()) {
      if (EQ$operator(this._normalListeners$$getter_(), $Dart$Null)) {
        this._normalListeners$$setter_(tmp$0 = DoubleLinkedQueue$Dart.DoubleLinkedQueue$$Factory(DoubleLinkedQueue$Dart.$lookupRTT([Function$Dart.$lookupRTT()]))) , tmp$0;
      }
      this._normalListeners$$getter_().addLast$named(1, $noargs, completeHandler);
    }
  }
}
;
PromiseImpl$Dart.prototype.addCompleteHandler$named = function($n, $o, completeHandler){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart.prototype.addCompleteHandler$member.call(this, completeHandler);
}
;
PromiseImpl$Dart.prototype.addCompleteHandler$getter = function addCompleteHandler$getter(){
  return $bind(PromiseImpl$Dart.prototype.addCompleteHandler$named, this);
}
;
PromiseImpl$Dart.prototype.addErrorHandler$member = function(errorHandler){
  var tmp$0;
  if (EQ$operator(this._state$$getter_(), PromiseImpl$Dart.COMPLETE_ERROR$getter())) {
    errorHandler(1, $noargs, this._error$$getter_());
  }
   else {
    if (!this.isDone$member()) {
      if (EQ$operator(this._errorListeners$$getter_(), $Dart$Null)) {
        this._errorListeners$$setter_(tmp$0 = DoubleLinkedQueue$Dart.DoubleLinkedQueue$$Factory(DoubleLinkedQueue$Dart.$lookupRTT([Function$Dart.$lookupRTT()]))) , tmp$0;
      }
      this._errorListeners$$getter_().addLast$named(1, $noargs, errorHandler);
    }
  }
}
;
PromiseImpl$Dart.prototype.addErrorHandler$named = function($n, $o, errorHandler){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart.prototype.addErrorHandler$member.call(this, errorHandler);
}
;
PromiseImpl$Dart.prototype.addErrorHandler$getter = function addErrorHandler$getter(){
  return $bind(PromiseImpl$Dart.prototype.addErrorHandler$named, this);
}
;
PromiseImpl$Dart.prototype.addCancelHandler$member = function(cancelHandler){
  var tmp$0;
  if (this.isCancelled$member()) {
    cancelHandler(0, $noargs);
  }
   else {
    if (!this.isDone$member()) {
      if (EQ$operator(this._cancelListeners$$getter_(), $Dart$Null)) {
        this._cancelListeners$$setter_(tmp$0 = DoubleLinkedQueue$Dart.DoubleLinkedQueue$$Factory(DoubleLinkedQueue$Dart.$lookupRTT([Function$Dart.$lookupRTT()]))) , tmp$0;
      }
      this._cancelListeners$$getter_().addLast$named(1, $noargs, cancelHandler);
    }
  }
}
;
PromiseImpl$Dart.prototype.addCancelHandler$named = function($n, $o, cancelHandler){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart.prototype.addCancelHandler$member.call(this, cancelHandler);
}
;
PromiseImpl$Dart.prototype.addCancelHandler$getter = function addCancelHandler$getter(){
  return $bind(PromiseImpl$Dart.prototype.addCancelHandler$named, this);
}
;
function PromiseImpl$Dart$then$c0$16_16$Hoisted(dartc_scp$0, dartc_scp$1, val){
  dartc_scp$1.promise.complete$named(1, $noargs, dartc_scp$0.callback(1, $noargs, val));
}

function PromiseImpl$Dart$then$c0$16_16$Hoisted$named($s0, $s1, $n, $o, val){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart$then$c0$16_16$Hoisted($s0, $s1, val);
}

function PromiseImpl$Dart$then$c1$16_16$Hoisted(dartc_scp$1, err){
  dartc_scp$1.promise.fail$named(1, $noargs, err);
}

function PromiseImpl$Dart$then$c1$16_16$Hoisted$named($s0, $n, $o, err){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart$then$c1$16_16$Hoisted($s0, err);
}

function PromiseImpl$Dart$then$c2$16_16$Hoisted(dartc_scp$1){
  dartc_scp$1.promise.fail$named(1, $noargs, 'Source promise was cancelled');
}

function PromiseImpl$Dart$then$c2$16_16$Hoisted$named($s0, $n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return PromiseImpl$Dart$then$c2$16_16$Hoisted($s0);
}

PromiseImpl$Dart.prototype.then$member = function(callback){
  var dartc_scp$0 = {callback:callback};
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.promise = PromiseImpl$Dart.PromiseImpl$$Factory(PromiseImpl$Dart.$lookupRTT());
  this.addCompleteHandler$member($bind(PromiseImpl$Dart$then$c0$16_16$Hoisted$named, $Dart$Null, dartc_scp$0, dartc_scp$1));
  this.addErrorHandler$member($bind(PromiseImpl$Dart$then$c1$16_16$Hoisted$named, $Dart$Null, dartc_scp$1));
  this.addCancelHandler$member($bind(PromiseImpl$Dart$then$c2$16_16$Hoisted$named, $Dart$Null, dartc_scp$1));
  return dartc_scp$1.promise;
  dartc_scp$1 = $Dart$Null;
}
;
PromiseImpl$Dart.prototype.then$named = function($n, $o, callback){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart.prototype.then$member.call(this, callback);
}
;
PromiseImpl$Dart.prototype.then$getter = function then$getter(){
  return $bind(PromiseImpl$Dart.prototype.then$named, this);
}
;
function PromiseImpl$Dart$flatten$c0$16_16$Hoisted(dartc_scp$1, lastVal){
  dartc_scp$1.res.complete$named(1, $noargs, lastVal);
}

function PromiseImpl$Dart$flatten$c0$16_16$Hoisted$named($s0, $n, $o, lastVal){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart$flatten$c0$16_16$Hoisted($s0, lastVal);
}

function PromiseImpl$Dart$flatten$c1$16_16$Hoisted(dartc_scp$1, thisVal){
  var tmp$0;
  if (!!(tmp$0 = thisVal , tmp$0 != null && tmp$0.$implements$Promise$Dart)) {
    var thisPromise = thisVal.dynamic$getter();
    thisPromise.flatten$named(0, $noargs).then$named(1, $noargs, $bind(PromiseImpl$Dart$flatten$c0$16_16$Hoisted$named, $Dart$Null, dartc_scp$1));
  }
   else {
    dartc_scp$1.res.complete$named(1, $noargs, thisVal);
  }
}

function PromiseImpl$Dart$flatten$c1$16_16$Hoisted$named($s0, $n, $o, thisVal){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart$flatten$c1$16_16$Hoisted($s0, thisVal);
}

PromiseImpl$Dart.prototype.flatten$member = function(){
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.res = PromiseImpl$Dart.PromiseImpl$$Factory(PromiseImpl$Dart.$lookupRTT());
  this.then$member($bind(PromiseImpl$Dart$flatten$c1$16_16$Hoisted$named, $Dart$Null, dartc_scp$1));
  return dartc_scp$1.res;
  dartc_scp$1 = $Dart$Null;
}
;
PromiseImpl$Dart.prototype.flatten$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return PromiseImpl$Dart.prototype.flatten$member.call(this);
}
;
PromiseImpl$Dart.prototype.flatten$getter = function flatten$getter(){
  return $bind(PromiseImpl$Dart.prototype.flatten$named, this);
}
;
function PromiseImpl$Dart$join$c0$16_16$Hoisted(dartc_scp$0, dartc_scp$3, value){
  if (dartc_scp$0.joinDone(1, $noargs, dartc_scp$3.promise)) {
    this.complete$member(value);
  }
}

function PromiseImpl$Dart$join$c0$16_16$Hoisted$named($s0, $s1, $n, $o, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart$join$c0$16_16$Hoisted.call(this, $s0, $s1, value);
}

function PromiseImpl$Dart$join$c1$16_16$Hoisted(err){
  this.fail$member(err);
}

function PromiseImpl$Dart$join$c1$16_16$Hoisted$named($n, $o, err){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart$join$c1$16_16$Hoisted.call(this, err);
}

function PromiseImpl$Dart$join$c2$16_16$Hoisted(dartc_scp$0, promise){
  var dartc_scp$3 = {promise:promise};
  dartc_scp$3.promise.addCompleteHandler$named(1, $noargs, $bind(PromiseImpl$Dart$join$c0$16_16$Hoisted$named, this, dartc_scp$0, dartc_scp$3));
  dartc_scp$3.promise.addErrorHandler$named(1, $noargs, $bind(PromiseImpl$Dart$join$c1$16_16$Hoisted$named, this));
}

function PromiseImpl$Dart$join$c2$16_16$Hoisted$named($s0, $n, $o, promise){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart$join$c2$16_16$Hoisted.call(this, $s0, promise);
}

function PromiseImpl$Dart$join$c3$16_16$Hoisted(promise){
  promise.cancel$named(0, $noargs);
}

function PromiseImpl$Dart$join$c3$16_16$Hoisted$named($n, $o, promise){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart$join$c3$16_16$Hoisted(promise);
}

function PromiseImpl$Dart$join$c4$16_16$Hoisted(dartc_scp$0){
  dartc_scp$0.promises.forEach$named(1, $noargs, $bind(PromiseImpl$Dart$join$c3$16_16$Hoisted$named, $Dart$Null));
}

function PromiseImpl$Dart$join$c4$16_16$Hoisted$named($s0, $n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return PromiseImpl$Dart$join$c4$16_16$Hoisted($s0);
}

PromiseImpl$Dart.prototype.join$member = function(promises, joinDone){
  var dartc_scp$0 = {promises:promises, joinDone:joinDone};
  dartc_scp$0.promises.forEach$named(1, $noargs, $bind(PromiseImpl$Dart$join$c2$16_16$Hoisted$named, this, dartc_scp$0));
  this.addCancelHandler$member($bind(PromiseImpl$Dart$join$c4$16_16$Hoisted$named, $Dart$Null, dartc_scp$0));
}
;
PromiseImpl$Dart.prototype.join$named = function($n, $o, promises, joinDone){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return PromiseImpl$Dart.prototype.join$member.call(this, promises, joinDone);
}
;
PromiseImpl$Dart.prototype.join$getter = function join$getter(){
  return $bind(PromiseImpl$Dart.prototype.join$named, this);
}
;
function PromiseImpl$Dart$waitFor$c0$16_16$Hoisted(dartc_scp$0, dartc_scp$1, p){
  return EQ$operator(dartc_scp$1.counter = ADD$operator(dartc_scp$1.counter, 1), dartc_scp$0.n);
}

function PromiseImpl$Dart$waitFor$c0$16_16$Hoisted$named($s0, $s1, $n, $o, p){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart$waitFor$c0$16_16$Hoisted($s0, $s1, p);
}

function PromiseImpl$Dart$waitFor$c1$16_16$Hoisted(promise){
  if (!promise.isDone$named(0, $noargs)) {
    promise.cancel$named(0, $noargs);
  }
}

function PromiseImpl$Dart$waitFor$c1$16_16$Hoisted$named($n, $o, promise){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart$waitFor$c1$16_16$Hoisted(promise);
}

function PromiseImpl$Dart$waitFor$c2$16_16$Hoisted(dartc_scp$0, val){
  dartc_scp$0.promises.forEach$named(1, $noargs, $bind(PromiseImpl$Dart$waitFor$c1$16_16$Hoisted$named, $Dart$Null));
}

function PromiseImpl$Dart$waitFor$c2$16_16$Hoisted$named($s0, $n, $o, val){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseImpl$Dart$waitFor$c2$16_16$Hoisted($s0, val);
}

PromiseImpl$Dart.prototype.waitFor$member = function(promises, n){
  var dartc_scp$0 = {promises:promises, n:n};
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.counter = 0;
  this.join$member(dartc_scp$0.promises, $bind(PromiseImpl$Dart$waitFor$c0$16_16$Hoisted$named, $Dart$Null, dartc_scp$0, dartc_scp$1));
  this.addCompleteHandler$member($bind(PromiseImpl$Dart$waitFor$c2$16_16$Hoisted$named, $Dart$Null, dartc_scp$0));
  dartc_scp$1 = $Dart$Null;
}
;
PromiseImpl$Dart.prototype.waitFor$named = function($n, $o, promises, n){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return PromiseImpl$Dart.prototype.waitFor$member.call(this, promises, n);
}
;
PromiseImpl$Dart.prototype.waitFor$getter = function waitFor$getter(){
  return $bind(PromiseImpl$Dart.prototype.waitFor$named, this);
}
;
function ProxyImpl$Dart(){
}

ProxyImpl$Dart.$lookupRTT = function(){
  return RTT.create($cls('ProxyImpl$Dart'));
}
;
ProxyImpl$Dart.$addTo = function(target){
  var rtt = ProxyImpl$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
ProxyImpl$Dart.prototype.$implements$ProxyImpl$Dart = 1;
ProxyImpl$Dart.prototype.$implements$Object$Dart = 1;
ProxyImpl$Dart.forPort$Constructor = function(port){
  Object.$Constructor.call(this);
  var tmp$0;
  this._promise$$setter_(tmp$0 = PromiseImpl$Dart.PromiseImpl$$Factory(PromiseImpl$Dart.$lookupRTT([SendPort$Dart.$lookupRTT()]))) , tmp$0;
  this._promise$$getter_().complete$named(1, $noargs, port);
}
;
ProxyImpl$Dart.forPort$Initializer = function(port){
  Object.$Initializer.call(this);
}
;
ProxyImpl$Dart.ProxyImpl$forPort$9$Factory = function(port){
  var tmp$0 = new ProxyImpl$Dart;
  tmp$0.$typeInfo = ProxyImpl$Dart.$lookupRTT();
  ProxyImpl$Dart.forPort$Initializer.call(tmp$0, port);
  ProxyImpl$Dart.forPort$Constructor.call(tmp$0, port);
  return tmp$0;
}
;
ProxyImpl$Dart.forReply$Constructor = function(port){
  Object.$Constructor.call(this);
  var tmp$0;
  this._promise$$setter_(tmp$0 = port) , tmp$0;
}
;
ProxyImpl$Dart.forReply$Initializer = function(port){
  Object.$Initializer.call(this);
}
;
ProxyImpl$Dart.ProxyImpl$forReply$9$Factory = function(port){
  var tmp$0 = new ProxyImpl$Dart;
  tmp$0.$typeInfo = ProxyImpl$Dart.$lookupRTT();
  ProxyImpl$Dart.forReply$Initializer.call(tmp$0, port);
  ProxyImpl$Dart.forReply$Constructor.call(tmp$0, port);
  return tmp$0;
}
;
ProxyImpl$Dart.register$member = function(dispatcher){
  var tmp$1, tmp$0;
  if (ProxyImpl$Dart._dispatchers$$getter_() == null) {
    ProxyImpl$Dart._dispatchers$$setter_(tmp$0 = HashMapImplementation$Dart.HashMapImplementation$$Factory(HashMapImplementation$Dart.$lookupRTT([SendPort$Dart.$lookupRTT(), Dispatcher$Dart.$lookupRTT()]))) , tmp$0;
  }
  var result = ReceivePortFactory$Dart.ReceivePort$$Factory();
  ProxyImpl$Dart._dispatchers$$getter_().ASSIGN_INDEX$operator(result.toSendPort$named(0, $noargs), tmp$1 = dispatcher) , tmp$1;
  return result;
}
;
ProxyImpl$Dart.register$named = function($n, $o, dispatcher){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return ProxyImpl$Dart.register$member(dispatcher);
}
;
ProxyImpl$Dart.register$getter = function register$getter(){
  return ProxyImpl$Dart.register$named;
}
;
ProxyImpl$Dart.prototype.local$named = function(){
  return this.local$getter().apply(this, arguments);
}
;
ProxyImpl$Dart.prototype.local$getter = function(){
  if (ProxyImpl$Dart._dispatchers$$getter_() != null) {
    var dispatcher = ProxyImpl$Dart._dispatchers$$getter_().INDEX$operator(this._promise$$getter_().value$getter());
    if (dispatcher != null) {
      return dispatcher.target$getter();
    }
  }
  $Dart$ThrowException('Cannot access object of non-local proxy.');
}
;
function ProxyImpl$Dart$send$c0$14_14$Hoisted(marshalled){
  var port = this._promise$$getter_().value$getter();
  port._sendNow$$named_(2, $noargs, marshalled, $Dart$Null);
}

function ProxyImpl$Dart$send$c0$14_14$Hoisted$named($n, $o, marshalled){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return ProxyImpl$Dart$send$c0$14_14$Hoisted.call(this, marshalled);
}

ProxyImpl$Dart.prototype.send$member = function(message){
  this._marshal$$member_(message, $bind(ProxyImpl$Dart$send$c0$14_14$Hoisted$named, this));
}
;
ProxyImpl$Dart.prototype.send$named = function($n, $o, message){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return ProxyImpl$Dart.prototype.send$member.call(this, message);
}
;
ProxyImpl$Dart.prototype.send$getter = function send$getter(){
  return $bind(ProxyImpl$Dart.prototype.send$named, this);
}
;
function ProxyImpl$Dart$call$c0$14_14$Hoisted(dartc_scp$4, message_0, replyTo){
  dartc_scp$4.result.complete$named(1, $noargs, message_0.INDEX$operator(0));
}

function ProxyImpl$Dart$call$c0$14_14$Hoisted$named($s0, $n, $o, message, replyTo){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return ProxyImpl$Dart$call$c0$14_14$Hoisted($s0, message, replyTo);
}

function ProxyImpl$Dart$call$c1$14_14$Hoisted(marshalled){
  var dartc_scp$4;
  dartc_scp$4 = {};
  dartc_scp$4.result = PromiseImpl$Dart.PromiseImpl$$Factory(PromiseImpl$Dart.$lookupRTT());
  var outgoing = this._promise$$getter_().value$getter();
  var incoming = outgoing._callNow$$named_(1, $noargs, marshalled);
  incoming.receive$named(1, $noargs, $bind(ProxyImpl$Dart$call$c0$14_14$Hoisted$named, $Dart$Null, dartc_scp$4));
  return dartc_scp$4.result;
  dartc_scp$4 = $Dart$Null;
}

function ProxyImpl$Dart$call$c1$14_14$Hoisted$named($n, $o, marshalled){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return ProxyImpl$Dart$call$c1$14_14$Hoisted.call(this, marshalled);
}

ProxyImpl$Dart.prototype.call$member = function(message){
  return this._marshal$$member_(message, $bind(ProxyImpl$Dart$call$c1$14_14$Hoisted$named, this));
}
;
ProxyImpl$Dart.prototype.call$named = function($n, $o, message){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return ProxyImpl$Dart.prototype.call$member.call(this, message);
}
;
ProxyImpl$Dart.prototype.call$getter = function call$getter(){
  return $bind(ProxyImpl$Dart.prototype.call$named, this);
}
;
ProxyImpl$Dart.prototype.EQ$operator = function(other){
  return this === other;
}
;
ProxyImpl$Dart.prototype.hashCode$member = function(){
  return 0;
}
;
ProxyImpl$Dart.prototype.hashCode$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return ProxyImpl$Dart.prototype.hashCode$member.call(this);
}
;
ProxyImpl$Dart.prototype.hashCode$getter = function hashCode$getter(){
  return $bind(ProxyImpl$Dart.prototype.hashCode$named, this);
}
;
function ProxyImpl$Dart$_marshal$c0$14_14$Hoisted(dartc_scp$0, dartc_scp$1, ignored){
  var tmp$1, tmp$2, tmp$3, tmp$4, tmp$0;
  {
    var i_0 = 0;
    for (; LT$operator(i_0, dartc_scp$1.marshalled_0.length$getter()); tmp$0 = i_0 , (i_0 = ADD$operator(tmp$0, 1) , tmp$0)) {
      var entry_0 = dartc_scp$1.marshalled_0.INDEX$operator(i_0);
      if (!!(tmp$1 = entry_0 , tmp$1 != null && tmp$1.$implements$Proxy$Dart)) {
        dartc_scp$1.marshalled_0.ASSIGN_INDEX$operator(i_0, tmp$2 = entry_0._promise$$getter_().value$getter()) , tmp$2;
      }
       else {
        if (!!(tmp$3 = entry_0 , tmp$3 != null && tmp$3.$implements$Promise$Dart)) {
          dartc_scp$1.marshalled_0.ASSIGN_INDEX$operator(i_0, tmp$4 = entry_0.value$getter()) , tmp$4;
        }
      }
    }
  }
  return dartc_scp$0.process(1, $noargs, dartc_scp$1.marshalled_0);
}

function ProxyImpl$Dart$_marshal$c0$14_14$Hoisted$named($s0, $s1, $n, $o, ignored){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return ProxyImpl$Dart$_marshal$c0$14_14$Hoisted($s0, $s1, ignored);
}

ProxyImpl$Dart.prototype._marshal$$member_ = function(message, process){
  var dartc_scp$0 = {process:process};
  var dartc_scp$1, tmp$1, tmp$2, tmp$3, tmp$0;
  dartc_scp$1 = {};
  var promises = ListFactory$Dart.List$$Factory([Promise$Dart.$lookupRTT()], $Dart$Null);
  promises.add$named(1, $noargs, this._promise$$getter_());
  dartc_scp$1.marshalled_0 = ListFactory$Dart.List$$Factory(null, message.length$getter());
  {
    var i = 0;
    for (; LT$operator(i, dartc_scp$1.marshalled_0.length$getter()); tmp$0 = i , (i = ADD$operator(tmp$0, 1) , tmp$0)) {
      var entry = message.INDEX$operator(i);
      dartc_scp$1.marshalled_0.ASSIGN_INDEX$operator(i, tmp$1 = entry) , tmp$1;
      if (!!(tmp$2 = entry , tmp$2 != null && tmp$2.$implements$Proxy$Dart)) {
        promises.add$named(1, $noargs, entry._promise$$getter_());
      }
       else {
        if (!!(tmp$3 = entry , tmp$3 != null && tmp$3.$implements$Promise$Dart)) {
          promises.add$named(1, $noargs, entry);
        }
      }
    }
  }
  return PromiseQueue$Dart.enqueue$member(promises).then$named(1, $noargs, $bind(ProxyImpl$Dart$_marshal$c0$14_14$Hoisted$named, $Dart$Null, dartc_scp$0, dartc_scp$1)).flatten$named(0, $noargs);
  dartc_scp$1 = $Dart$Null;
}
;
ProxyImpl$Dart.prototype._marshal$$named_ = function($n, $o, message, process){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return ProxyImpl$Dart.prototype._marshal$$member_.call(this, message, process);
}
;
ProxyImpl$Dart.prototype._marshal$$getter_ = function _marshal$$getter_(){
  return $bind(ProxyImpl$Dart.prototype._marshal$$named_, this);
}
;
ProxyImpl$Dart.prototype._promise$$named_ = function(){
  return this._promise$$getter_().apply(this, arguments);
}
;
ProxyImpl$Dart.prototype._promise$$getter_ = function(){
  return this._promise$$field_;
}
;
ProxyImpl$Dart.prototype._promise$$setter_ = function(tmp$0){
  this._promise$$field_ = tmp$0;
}
;
ProxyImpl$Dart._dispatchers$$named_ = function(){
  return ProxyImpl$Dart._dispatchers$$getter_().apply(this, arguments);
}
;
ProxyImpl$Dart._dispatchers$$getter_ = function(){
  return isolate$current.ProxyImpl$Dart_dispatchers$$field_;
}
;
ProxyImpl$Dart._dispatchers$$setter_ = function(tmp$0){
  isolate$current.ProxyImpl$Dart_dispatchers$$field_ = tmp$0;
}
;
function PromiseQueue$Dart(){
}

PromiseQueue$Dart.$lookupRTT = function(){
  return RTT.create($cls('PromiseQueue$Dart'));
}
;
PromiseQueue$Dart.$addTo = function(target){
  var rtt = PromiseQueue$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
PromiseQueue$Dart.prototype.$implements$PromiseQueue$Dart = 1;
PromiseQueue$Dart.prototype.$implements$Object$Dart = 1;
function PromiseQueue$Dart$enqueue$c0$notifyResolved$17_7_2$Hoisted(dartc_scp$1, ignored){
  var tmp$0;
  assert(GT$operator(dartc_scp$1.unresolved, 0));
  tmp$0 = dartc_scp$1.unresolved , (dartc_scp$1.unresolved = SUB$operator(tmp$0, 1) , tmp$0);
}

function PromiseQueue$Dart$enqueue$c0$notifyResolved$17_7_2$Hoisted$named($s0, $n, $o, ignored){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseQueue$Dart$enqueue$c0$notifyResolved$17_7_2$Hoisted($s0, ignored);
}

function PromiseQueue$Dart$enqueue$c1$17_17$Hoisted(dartc_scp$1){
  if (GT$operator(dartc_scp$1.unresolved, 0)) {
    return false;
  }
  dartc_scp$1.result.complete$named(1, $noargs, $Dart$Null);
  return true;
}

function PromiseQueue$Dart$enqueue$c1$17_17$Hoisted$named($s0, $n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return PromiseQueue$Dart$enqueue$c1$17_17$Hoisted($s0);
}

PromiseQueue$Dart.enqueue$member = function(dependencies){
  var dartc_scp$1, tmp$0;
  dartc_scp$1 = {};
  if (PromiseQueue$Dart._queue$$getter_() == null) {
    PromiseQueue$Dart._queue$$setter_(tmp$0 = DoubleLinkedQueue$Dart.DoubleLinkedQueue$$Factory(DoubleLinkedQueue$Dart.$lookupRTT([Function$Dart.$lookupRTT()]))) , tmp$0;
  }
  dartc_scp$1.unresolved = dependencies.length$getter();
  var notifyResolved = $bind(PromiseQueue$Dart$enqueue$c0$notifyResolved$17_7_2$Hoisted$named, $Dart$Null, dartc_scp$1);
  {
    var $1 = dependencies.iterator$named(0, $noargs);
    while ($1.hasNext$named(0, $noargs)) {
      var promise = $1.next$named(0, $noargs);
      {
        promise.then$named(1, $noargs, notifyResolved);
      }
    }
  }
  dartc_scp$1.result = PromiseImpl$Dart.PromiseImpl$$Factory(PromiseImpl$Dart.$lookupRTT());
  PromiseQueue$Dart._queue$$getter_().addLast$named(1, $noargs, $bind(PromiseQueue$Dart$enqueue$c1$17_17$Hoisted$named, $Dart$Null, dartc_scp$1));
  PromiseQueue$Dart.process$member();
  return dartc_scp$1.result;
  dartc_scp$1 = $Dart$Null;
}
;
PromiseQueue$Dart.enqueue$named = function($n, $o, dependencies){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return PromiseQueue$Dart.enqueue$member(dependencies);
}
;
PromiseQueue$Dart.enqueue$getter = function enqueue$getter(){
  return PromiseQueue$Dart.enqueue$named;
}
;
PromiseQueue$Dart.isEmpty$member = function(){
  return PromiseQueue$Dart._queue$$getter_() == null?true:PromiseQueue$Dart._queue$$getter_().isEmpty$named(0, $noargs);
}
;
PromiseQueue$Dart.isEmpty$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return PromiseQueue$Dart.isEmpty$member();
}
;
PromiseQueue$Dart.isEmpty$getter = function isEmpty$getter(){
  return PromiseQueue$Dart.isEmpty$named;
}
;
PromiseQueue$Dart.process$member = function(){
  if (PromiseQueue$Dart._queue$$getter_() == null) {
    return;
  }
  while (!PromiseQueue$Dart._queue$$getter_().isEmpty$named(0, $noargs) && PromiseQueue$Dart._queue$$getter_().first$named(0, $noargs)(0, $noargs)) {
    PromiseQueue$Dart._queue$$getter_().removeFirst$named(0, $noargs);
  }
}
;
PromiseQueue$Dart.process$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return PromiseQueue$Dart.process$member();
}
;
PromiseQueue$Dart.process$getter = function process$getter(){
  return PromiseQueue$Dart.process$named;
}
;
PromiseQueue$Dart._queue$$named_ = function(){
  return PromiseQueue$Dart._queue$$getter_().apply(this, arguments);
}
;
PromiseQueue$Dart._queue$$getter_ = function(){
  return isolate$current.PromiseQueue$Dart_queue$$field_;
}
;
PromiseQueue$Dart._queue$$setter_ = function(tmp$0){
  isolate$current.PromiseQueue$Dart_queue$$field_ = tmp$0;
}
;
PromiseQueue$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
PromiseQueue$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
PromiseQueue$Dart.PromiseQueue$$Factory = function(){
  var tmp$0 = new PromiseQueue$Dart;
  tmp$0.$typeInfo = PromiseQueue$Dart.$lookupRTT();
  PromiseQueue$Dart.$Initializer.call(tmp$0);
  PromiseQueue$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function DoubleLinkedQueueEntry$Dart(){
}

DoubleLinkedQueueEntry$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('DoubleLinkedQueueEntry$Dart'), null, typeArgs);
}
;
DoubleLinkedQueueEntry$Dart.$addTo = function(target, typeArgs){
  var rtt = DoubleLinkedQueueEntry$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
}
;
DoubleLinkedQueueEntry$Dart.prototype.$implements$DoubleLinkedQueueEntry$Dart = 1;
DoubleLinkedQueueEntry$Dart.prototype.$implements$Object$Dart = 1;
DoubleLinkedQueueEntry$Dart.$Constructor = function(e){
  Object.$Constructor.call(this);
  var tmp$0;
  this._element$$setter_(tmp$0 = e) , tmp$0;
}
;
DoubleLinkedQueueEntry$Dart.$Initializer = function(e){
  Object.$Initializer.call(this);
}
;
DoubleLinkedQueueEntry$Dart.DoubleLinkedQueueEntry$$Factory = function($rtt, e){
  var tmp$0 = new DoubleLinkedQueueEntry$Dart;
  tmp$0.$typeInfo = $rtt;
  DoubleLinkedQueueEntry$Dart.$Initializer.call(tmp$0, e);
  DoubleLinkedQueueEntry$Dart.$Constructor.call(tmp$0, e);
  return tmp$0;
}
;
DoubleLinkedQueueEntry$Dart.prototype._previous$$named_ = function(){
  return this._previous$$getter_().apply(this, arguments);
}
;
DoubleLinkedQueueEntry$Dart.prototype._previous$$getter_ = function(){
  return this._previous$$field_;
}
;
DoubleLinkedQueueEntry$Dart.prototype._previous$$setter_ = function(tmp$0){
  this._previous$$field_ = tmp$0;
}
;
DoubleLinkedQueueEntry$Dart.prototype._next$$named_ = function(){
  return this._next$$getter_().apply(this, arguments);
}
;
DoubleLinkedQueueEntry$Dart.prototype._next$$getter_ = function(){
  return this._next$$field_;
}
;
DoubleLinkedQueueEntry$Dart.prototype._next$$setter_ = function(tmp$0){
  this._next$$field_ = tmp$0;
}
;
DoubleLinkedQueueEntry$Dart.prototype._element$$named_ = function(){
  return this._element$$getter_().apply(this, arguments);
}
;
DoubleLinkedQueueEntry$Dart.prototype._element$$getter_ = function(){
  return this._element$$field_;
}
;
DoubleLinkedQueueEntry$Dart.prototype._element$$setter_ = function(tmp$0){
  this._element$$field_ = tmp$0;
}
;
DoubleLinkedQueueEntry$Dart.prototype._link$$member_ = function(p, n){
  var tmp$1, tmp$2, tmp$3, tmp$0;
  this._next$$setter_(tmp$0 = n) , tmp$0;
  this._previous$$setter_(tmp$1 = p) , tmp$1;
  p._next$$setter_(tmp$2 = this) , tmp$2;
  n._previous$$setter_(tmp$3 = this) , tmp$3;
}
;
DoubleLinkedQueueEntry$Dart.prototype._link$$named_ = function($n, $o, p, n){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return DoubleLinkedQueueEntry$Dart.prototype._link$$member_.call(this, p, n);
}
;
DoubleLinkedQueueEntry$Dart.prototype._link$$getter_ = function _link$$getter_(){
  return $bind(DoubleLinkedQueueEntry$Dart.prototype._link$$named_, this);
}
;
DoubleLinkedQueueEntry$Dart.prototype.append$member = function(e){
  DoubleLinkedQueueEntry$Dart.DoubleLinkedQueueEntry$$Factory(DoubleLinkedQueueEntry$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('DoubleLinkedQueueEntry$Dart')), 0)]), e)._link$$named_(2, $noargs, this, this._next$$getter_());
}
;
DoubleLinkedQueueEntry$Dart.prototype.append$named = function($n, $o, e){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DoubleLinkedQueueEntry$Dart.prototype.append$member.call(this, e);
}
;
DoubleLinkedQueueEntry$Dart.prototype.append$getter = function append$getter(){
  return $bind(DoubleLinkedQueueEntry$Dart.prototype.append$named, this);
}
;
DoubleLinkedQueueEntry$Dart.prototype.prepend$member = function(e){
  DoubleLinkedQueueEntry$Dart.DoubleLinkedQueueEntry$$Factory(DoubleLinkedQueueEntry$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('DoubleLinkedQueueEntry$Dart')), 0)]), e)._link$$named_(2, $noargs, this._previous$$getter_(), this);
}
;
DoubleLinkedQueueEntry$Dart.prototype.prepend$named = function($n, $o, e){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DoubleLinkedQueueEntry$Dart.prototype.prepend$member.call(this, e);
}
;
DoubleLinkedQueueEntry$Dart.prototype.prepend$getter = function prepend$getter(){
  return $bind(DoubleLinkedQueueEntry$Dart.prototype.prepend$named, this);
}
;
DoubleLinkedQueueEntry$Dart.prototype.remove$member = function(){
  var tmp$1, tmp$2, tmp$3, tmp$0;
  this._previous$$getter_()._next$$setter_(tmp$0 = this._next$$getter_()) , tmp$0;
  this._next$$getter_()._previous$$setter_(tmp$1 = this._previous$$getter_()) , tmp$1;
  this._next$$setter_(tmp$2 = $Dart$Null) , tmp$2;
  this._previous$$setter_(tmp$3 = $Dart$Null) , tmp$3;
  return this._element$$getter_();
}
;
DoubleLinkedQueueEntry$Dart.prototype.remove$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DoubleLinkedQueueEntry$Dart.prototype.remove$member.call(this);
}
;
DoubleLinkedQueueEntry$Dart.prototype.remove$getter = function remove$getter(){
  return $bind(DoubleLinkedQueueEntry$Dart.prototype.remove$named, this);
}
;
DoubleLinkedQueueEntry$Dart.prototype._asNonSentinelEntry$$member_ = function(){
  return this;
}
;
DoubleLinkedQueueEntry$Dart.prototype._asNonSentinelEntry$$named_ = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DoubleLinkedQueueEntry$Dart.prototype._asNonSentinelEntry$$member_.call(this);
}
;
DoubleLinkedQueueEntry$Dart.prototype._asNonSentinelEntry$$getter_ = function _asNonSentinelEntry$$getter_(){
  return $bind(DoubleLinkedQueueEntry$Dart.prototype._asNonSentinelEntry$$named_, this);
}
;
DoubleLinkedQueueEntry$Dart.prototype.previousEntry$member = function(){
  return this._previous$$getter_()._asNonSentinelEntry$$named_(0, $noargs);
}
;
DoubleLinkedQueueEntry$Dart.prototype.previousEntry$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DoubleLinkedQueueEntry$Dart.prototype.previousEntry$member.call(this);
}
;
DoubleLinkedQueueEntry$Dart.prototype.previousEntry$getter = function previousEntry$getter(){
  return $bind(DoubleLinkedQueueEntry$Dart.prototype.previousEntry$named, this);
}
;
DoubleLinkedQueueEntry$Dart.prototype.nextEntry$member = function(){
  return this._next$$getter_()._asNonSentinelEntry$$named_(0, $noargs);
}
;
DoubleLinkedQueueEntry$Dart.prototype.nextEntry$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DoubleLinkedQueueEntry$Dart.prototype.nextEntry$member.call(this);
}
;
DoubleLinkedQueueEntry$Dart.prototype.nextEntry$getter = function nextEntry$getter(){
  return $bind(DoubleLinkedQueueEntry$Dart.prototype.nextEntry$named, this);
}
;
DoubleLinkedQueueEntry$Dart.prototype.element$named = function(){
  return this.element$getter().apply(this, arguments);
}
;
DoubleLinkedQueueEntry$Dart.prototype.element$getter = function(){
  return this._element$$getter_();
}
;
DoubleLinkedQueueEntry$Dart.prototype.element$setter = function(e){
  var tmp$0;
  this._element$$setter_(tmp$0 = e) , tmp$0;
}
;
function _DoubleLinkedQueueEntrySentinel$Dart(){
}

_DoubleLinkedQueueEntrySentinel$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('_DoubleLinkedQueueEntrySentinel$Dart'), _DoubleLinkedQueueEntrySentinel$Dart.$RTTimplements, typeArgs);
}
;
_DoubleLinkedQueueEntrySentinel$Dart.$RTTimplements = function(rtt, typeArgs){
  _DoubleLinkedQueueEntrySentinel$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
_DoubleLinkedQueueEntrySentinel$Dart.$addTo = function(target, typeArgs){
  var rtt = _DoubleLinkedQueueEntrySentinel$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  DoubleLinkedQueueEntry$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0)]);
}
;
_DoubleLinkedQueueEntrySentinel$Dart.prototype.$implements$_DoubleLinkedQueueEntrySentinel$Dart = 1;
_DoubleLinkedQueueEntrySentinel$Dart.prototype.$implements$DoubleLinkedQueueEntry$Dart = 1;
_DoubleLinkedQueueEntrySentinel$Dart.prototype.$implements$Object$Dart = 1;
$inherits(_DoubleLinkedQueueEntrySentinel$Dart, DoubleLinkedQueueEntry$Dart);
_DoubleLinkedQueueEntrySentinel$Dart.$Constructor = function(){
  DoubleLinkedQueueEntry$Dart.$Constructor.call(this, $Dart$Null);
  this._link$$member_(this, this);
}
;
_DoubleLinkedQueueEntrySentinel$Dart.$Initializer = function(){
  DoubleLinkedQueueEntry$Dart.$Initializer.call(this, $Dart$Null);
}
;
_DoubleLinkedQueueEntrySentinel$Dart._DoubleLinkedQueueEntrySentinel$$Factory = function($rtt){
  var tmp$0 = new _DoubleLinkedQueueEntrySentinel$Dart;
  tmp$0.$typeInfo = $rtt;
  _DoubleLinkedQueueEntrySentinel$Dart.$Initializer.call(tmp$0);
  _DoubleLinkedQueueEntrySentinel$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
_DoubleLinkedQueueEntrySentinel$Dart.prototype.remove$member = function(){
  $Dart$ThrowException($intern(EmptyQueueException$Dart.EmptyQueueException$$Factory()));
}
;
_DoubleLinkedQueueEntrySentinel$Dart.prototype.remove$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return _DoubleLinkedQueueEntrySentinel$Dart.prototype.remove$member.call(this);
}
;
_DoubleLinkedQueueEntrySentinel$Dart.prototype.remove$getter = function remove$getter(){
  return $bind(_DoubleLinkedQueueEntrySentinel$Dart.prototype.remove$named, this);
}
;
_DoubleLinkedQueueEntrySentinel$Dart.prototype._asNonSentinelEntry$$member_ = function(){
  return $Dart$Null;
}
;
_DoubleLinkedQueueEntrySentinel$Dart.prototype._asNonSentinelEntry$$named_ = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return _DoubleLinkedQueueEntrySentinel$Dart.prototype._asNonSentinelEntry$$member_.call(this);
}
;
_DoubleLinkedQueueEntrySentinel$Dart.prototype._asNonSentinelEntry$$getter_ = function _asNonSentinelEntry$$getter_(){
  return $bind(_DoubleLinkedQueueEntrySentinel$Dart.prototype._asNonSentinelEntry$$named_, this);
}
;
_DoubleLinkedQueueEntrySentinel$Dart.prototype.element$named = function(){
  return this.element$getter().apply(this, arguments);
}
;
_DoubleLinkedQueueEntrySentinel$Dart.prototype.element$getter = function(){
  $Dart$ThrowException($intern(EmptyQueueException$Dart.EmptyQueueException$$Factory()));
}
;
_DoubleLinkedQueueEntrySentinel$Dart.prototype.element$setter = function(e){
  assert(false);
}
;
function DoubleLinkedQueue$Dart(){
}

DoubleLinkedQueue$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('DoubleLinkedQueue$Dart'), DoubleLinkedQueue$Dart.$RTTimplements, typeArgs);
}
;
DoubleLinkedQueue$Dart.$RTTimplements = function(rtt, typeArgs){
  DoubleLinkedQueue$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
DoubleLinkedQueue$Dart.$addTo = function(target, typeArgs){
  var rtt = DoubleLinkedQueue$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  Queue$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0)]);
}
;
DoubleLinkedQueue$Dart.prototype.$implements$DoubleLinkedQueue$Dart = 1;
DoubleLinkedQueue$Dart.prototype.$implements$Queue$Dart = 1;
DoubleLinkedQueue$Dart.prototype.$implements$Collection$Dart = 1;
DoubleLinkedQueue$Dart.prototype.$implements$Iterable$Dart = 1;
DoubleLinkedQueue$Dart.prototype.$implements$Object$Dart = 1;
DoubleLinkedQueue$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
  var tmp$0;
  this._sentinel$$setter_(tmp$0 = _DoubleLinkedQueueEntrySentinel$Dart._DoubleLinkedQueueEntrySentinel$$Factory(_DoubleLinkedQueueEntrySentinel$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('DoubleLinkedQueue$Dart')), 0)]))) , tmp$0;
}
;
DoubleLinkedQueue$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
DoubleLinkedQueue$Dart.DoubleLinkedQueue$$Factory = function($rtt){
  var tmp$0 = new DoubleLinkedQueue$Dart;
  tmp$0.$typeInfo = $rtt;
  DoubleLinkedQueue$Dart.$Initializer.call(tmp$0);
  DoubleLinkedQueue$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
DoubleLinkedQueue$Dart.DoubleLinkedQueue$from$17$Factory = function(other){
  var list = DoubleLinkedQueue$Dart.DoubleLinkedQueue$$Factory(DoubleLinkedQueue$Dart.$lookupRTT());
  {
    var $0 = other.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var e = $0.next$named(0, $noargs);
      {
        list.addLast$named(1, $noargs, e);
      }
    }
  }
  return list;
}
;
DoubleLinkedQueue$Dart.prototype._sentinel$$named_ = function(){
  return this._sentinel$$getter_().apply(this, arguments);
}
;
DoubleLinkedQueue$Dart.prototype._sentinel$$getter_ = function(){
  return this._sentinel$$field_;
}
;
DoubleLinkedQueue$Dart.prototype._sentinel$$setter_ = function(tmp$0){
  this._sentinel$$field_ = tmp$0;
}
;
DoubleLinkedQueue$Dart.prototype.addLast$member = function(value){
  this._sentinel$$getter_().prepend$named(1, $noargs, value);
}
;
DoubleLinkedQueue$Dart.prototype.addLast$named = function($n, $o, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.addLast$member.call(this, value);
}
;
DoubleLinkedQueue$Dart.prototype.addLast$getter = function addLast$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.addLast$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.addFirst$member = function(value){
  this._sentinel$$getter_().append$named(1, $noargs, value);
}
;
DoubleLinkedQueue$Dart.prototype.addFirst$named = function($n, $o, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.addFirst$member.call(this, value);
}
;
DoubleLinkedQueue$Dart.prototype.addFirst$getter = function addFirst$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.addFirst$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.add$member = function(value){
  this.addLast$member(value);
}
;
DoubleLinkedQueue$Dart.prototype.add$named = function($n, $o, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.add$member.call(this, value);
}
;
DoubleLinkedQueue$Dart.prototype.add$getter = function add$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.add$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.addAll$member = function(collection){
  {
    var $0 = collection.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var e = $0.next$named(0, $noargs);
      {
        this.add$member(e);
      }
    }
  }
}
;
DoubleLinkedQueue$Dart.prototype.addAll$named = function($n, $o, collection){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.addAll$member.call(this, collection);
}
;
DoubleLinkedQueue$Dart.prototype.addAll$getter = function addAll$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.addAll$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.removeLast$member = function(){
  return this._sentinel$$getter_()._previous$$getter_().remove$named(0, $noargs);
}
;
DoubleLinkedQueue$Dart.prototype.removeLast$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.removeLast$member.call(this);
}
;
DoubleLinkedQueue$Dart.prototype.removeLast$getter = function removeLast$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.removeLast$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.removeFirst$member = function(){
  return this._sentinel$$getter_()._next$$getter_().remove$named(0, $noargs);
}
;
DoubleLinkedQueue$Dart.prototype.removeFirst$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.removeFirst$member.call(this);
}
;
DoubleLinkedQueue$Dart.prototype.removeFirst$getter = function removeFirst$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.removeFirst$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.first$member = function(){
  return this._sentinel$$getter_()._next$$getter_().element$getter();
}
;
DoubleLinkedQueue$Dart.prototype.first$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.first$member.call(this);
}
;
DoubleLinkedQueue$Dart.prototype.first$getter = function first$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.first$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.last$member = function(){
  return this._sentinel$$getter_()._previous$$getter_().element$getter();
}
;
DoubleLinkedQueue$Dart.prototype.last$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.last$member.call(this);
}
;
DoubleLinkedQueue$Dart.prototype.last$getter = function last$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.last$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.lastEntry$member = function(){
  return this._sentinel$$getter_().previousEntry$named(0, $noargs);
}
;
DoubleLinkedQueue$Dart.prototype.lastEntry$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.lastEntry$member.call(this);
}
;
DoubleLinkedQueue$Dart.prototype.lastEntry$getter = function lastEntry$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.lastEntry$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.firstEntry$member = function(){
  return this._sentinel$$getter_().nextEntry$named(0, $noargs);
}
;
DoubleLinkedQueue$Dart.prototype.firstEntry$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.firstEntry$member.call(this);
}
;
DoubleLinkedQueue$Dart.prototype.firstEntry$getter = function firstEntry$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.firstEntry$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.length$named = function(){
  return this.length$getter().apply(this, arguments);
}
;
function DoubleLinkedQueue$Dart$length$c0$_$22_6_2$Hoisted(dartc_scp$1, element){
  var tmp$0;
  tmp$0 = dartc_scp$1.counter , (dartc_scp$1.counter = ADD$operator(tmp$0, 1) , tmp$0);
}

function DoubleLinkedQueue$Dart$length$c0$_$22_6_2$Hoisted$named($s0, $n, $o, element){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DoubleLinkedQueue$Dart$length$c0$_$22_6_2$Hoisted($s0, element);
}

DoubleLinkedQueue$Dart.prototype.length$getter = function(){
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.counter = 0;
  this.forEach$member($bind(DoubleLinkedQueue$Dart$length$c0$_$22_6_2$Hoisted$named, $Dart$Null, dartc_scp$1));
  return dartc_scp$1.counter;
  dartc_scp$1 = $Dart$Null;
}
;
DoubleLinkedQueue$Dart.prototype.isEmpty$member = function(){
  return this._sentinel$$getter_()._next$$getter_() === this._sentinel$$getter_();
}
;
DoubleLinkedQueue$Dart.prototype.isEmpty$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.isEmpty$member.call(this);
}
;
DoubleLinkedQueue$Dart.prototype.isEmpty$getter = function isEmpty$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.isEmpty$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.clear$member = function(){
  var tmp$1, tmp$0;
  this._sentinel$$getter_()._next$$setter_(tmp$0 = this._sentinel$$getter_()) , tmp$0;
  this._sentinel$$getter_()._previous$$setter_(tmp$1 = this._sentinel$$getter_()) , tmp$1;
}
;
DoubleLinkedQueue$Dart.prototype.clear$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.clear$member.call(this);
}
;
DoubleLinkedQueue$Dart.prototype.clear$getter = function clear$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.clear$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.forEach$member = function(f){
  var entry = this._sentinel$$getter_()._next$$getter_();
  while (entry !== this._sentinel$$getter_()) {
    f(1, $noargs, entry._element$$getter_());
    entry = entry._next$$getter_();
  }
}
;
DoubleLinkedQueue$Dart.prototype.forEach$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.forEach$member.call(this, f);
}
;
DoubleLinkedQueue$Dart.prototype.forEach$getter = function forEach$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.forEach$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.forEachEntry$member = function(f){
  var entry = this._sentinel$$getter_()._next$$getter_();
  while (entry !== this._sentinel$$getter_()) {
    f(1, $noargs, entry);
    entry = entry._next$$getter_();
  }
}
;
DoubleLinkedQueue$Dart.prototype.forEachEntry$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.forEachEntry$member.call(this, f);
}
;
DoubleLinkedQueue$Dart.prototype.forEachEntry$getter = function forEachEntry$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.forEachEntry$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.every$member = function(f){
  var entry = this._sentinel$$getter_()._next$$getter_();
  while (entry !== this._sentinel$$getter_()) {
    if (!f(1, $noargs, entry._element$$getter_())) {
      return false;
    }
    entry = entry._next$$getter_();
  }
  return true;
}
;
DoubleLinkedQueue$Dart.prototype.every$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.every$member.call(this, f);
}
;
DoubleLinkedQueue$Dart.prototype.every$getter = function every$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.every$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.some$member = function(f){
  var entry = this._sentinel$$getter_()._next$$getter_();
  while (entry !== this._sentinel$$getter_()) {
    if (f(1, $noargs, entry._element$$getter_())) {
      return true;
    }
    entry = entry._next$$getter_();
  }
  return false;
}
;
DoubleLinkedQueue$Dart.prototype.some$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.some$member.call(this, f);
}
;
DoubleLinkedQueue$Dart.prototype.some$getter = function some$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.some$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.filter$member = function(f){
  var other = DoubleLinkedQueue$Dart.DoubleLinkedQueue$$Factory(DoubleLinkedQueue$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('DoubleLinkedQueue$Dart')), 0)]));
  var entry = this._sentinel$$getter_()._next$$getter_();
  while (entry !== this._sentinel$$getter_()) {
    if (f(1, $noargs, entry._element$$getter_())) {
      other.addLast$named(1, $noargs, entry._element$$getter_());
    }
    entry = entry._next$$getter_();
  }
  return other;
}
;
DoubleLinkedQueue$Dart.prototype.filter$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.filter$member.call(this, f);
}
;
DoubleLinkedQueue$Dart.prototype.filter$getter = function filter$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.filter$named, this);
}
;
DoubleLinkedQueue$Dart.prototype.iterator$member = function(){
  return _DoubleLinkedQueueIterator$Dart._DoubleLinkedQueueIterator$$Factory(_DoubleLinkedQueueIterator$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('DoubleLinkedQueue$Dart')), 0)]), this._sentinel$$getter_());
}
;
DoubleLinkedQueue$Dart.prototype.iterator$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return DoubleLinkedQueue$Dart.prototype.iterator$member.call(this);
}
;
DoubleLinkedQueue$Dart.prototype.iterator$getter = function iterator$getter(){
  return $bind(DoubleLinkedQueue$Dart.prototype.iterator$named, this);
}
;
function _DoubleLinkedQueueIterator$Dart(){
}

_DoubleLinkedQueueIterator$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('_DoubleLinkedQueueIterator$Dart'), _DoubleLinkedQueueIterator$Dart.$RTTimplements, typeArgs);
}
;
_DoubleLinkedQueueIterator$Dart.$RTTimplements = function(rtt, typeArgs){
  _DoubleLinkedQueueIterator$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
_DoubleLinkedQueueIterator$Dart.$addTo = function(target, typeArgs){
  var rtt = _DoubleLinkedQueueIterator$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  Iterator$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0)]);
}
;
_DoubleLinkedQueueIterator$Dart.prototype.$implements$_DoubleLinkedQueueIterator$Dart = 1;
_DoubleLinkedQueueIterator$Dart.prototype.$implements$Iterator$Dart = 1;
_DoubleLinkedQueueIterator$Dart.prototype.$implements$Object$Dart = 1;
_DoubleLinkedQueueIterator$Dart.$Constructor = function(_sentinel){
  Object.$Constructor.call(this);
  var tmp$0;
  this._currentEntry$$setter_(tmp$0 = this._sentinel$$getter_()) , tmp$0;
}
;
_DoubleLinkedQueueIterator$Dart.$Initializer = function(_sentinel){
  Object.$Initializer.call(this);
  this._sentinel$$field_ = _sentinel;
}
;
_DoubleLinkedQueueIterator$Dart._DoubleLinkedQueueIterator$$Factory = function($rtt, _sentinel){
  var tmp$0 = new _DoubleLinkedQueueIterator$Dart;
  tmp$0.$typeInfo = $rtt;
  _DoubleLinkedQueueIterator$Dart.$Initializer.call(tmp$0, _sentinel);
  _DoubleLinkedQueueIterator$Dart.$Constructor.call(tmp$0, _sentinel);
  return tmp$0;
}
;
_DoubleLinkedQueueIterator$Dart.prototype._sentinel$$named_ = function(){
  return this._sentinel$$getter_().apply(this, arguments);
}
;
_DoubleLinkedQueueIterator$Dart.prototype._sentinel$$getter_ = function(){
  return this._sentinel$$field_;
}
;
_DoubleLinkedQueueIterator$Dart.prototype._currentEntry$$named_ = function(){
  return this._currentEntry$$getter_().apply(this, arguments);
}
;
_DoubleLinkedQueueIterator$Dart.prototype._currentEntry$$getter_ = function(){
  return this._currentEntry$$field_;
}
;
_DoubleLinkedQueueIterator$Dart.prototype._currentEntry$$setter_ = function(tmp$0){
  this._currentEntry$$field_ = tmp$0;
}
;
_DoubleLinkedQueueIterator$Dart.prototype.hasNext$member = function(){
  return this._currentEntry$$getter_()._next$$getter_() !== this._sentinel$$getter_();
}
;
_DoubleLinkedQueueIterator$Dart.prototype.hasNext$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return _DoubleLinkedQueueIterator$Dart.prototype.hasNext$member.call(this);
}
;
_DoubleLinkedQueueIterator$Dart.prototype.hasNext$getter = function hasNext$getter(){
  return $bind(_DoubleLinkedQueueIterator$Dart.prototype.hasNext$named, this);
}
;
_DoubleLinkedQueueIterator$Dart.prototype.next$member = function(){
  var tmp$0;
  if (!this.hasNext$member()) {
    $Dart$ThrowException($intern(NoMoreElementsException$Dart.NoMoreElementsException$$Factory()));
  }
  this._currentEntry$$setter_(tmp$0 = this._currentEntry$$getter_()._next$$getter_()) , tmp$0;
  return this._currentEntry$$getter_().element$getter();
}
;
_DoubleLinkedQueueIterator$Dart.prototype.next$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return _DoubleLinkedQueueIterator$Dart.prototype.next$member.call(this);
}
;
_DoubleLinkedQueueIterator$Dart.prototype.next$getter = function next$getter(){
  return $bind(_DoubleLinkedQueueIterator$Dart.prototype.next$named, this);
}
;
function SplayTreeNode$Dart(){
}

SplayTreeNode$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('SplayTreeNode$Dart'), null, typeArgs);
}
;
SplayTreeNode$Dart.$addTo = function(target, typeArgs){
  var rtt = SplayTreeNode$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
}
;
SplayTreeNode$Dart.prototype.$implements$SplayTreeNode$Dart = 1;
SplayTreeNode$Dart.prototype.$implements$Object$Dart = 1;
SplayTreeNode$Dart.$Constructor = function(k, v){
  Object.$Constructor.call(this);
  var tmp$1, tmp$0;
  this.key$setter(tmp$0 = k) , tmp$0;
  this.value$setter(tmp$1 = v) , tmp$1;
}
;
SplayTreeNode$Dart.$Initializer = function(k, v){
  Object.$Initializer.call(this);
}
;
SplayTreeNode$Dart.SplayTreeNode$$Factory = function($rtt, k, v){
  var tmp$0 = new SplayTreeNode$Dart;
  tmp$0.$typeInfo = $rtt;
  SplayTreeNode$Dart.$Initializer.call(tmp$0, k, v);
  SplayTreeNode$Dart.$Constructor.call(tmp$0, k, v);
  return tmp$0;
}
;
SplayTreeNode$Dart.prototype.key$named = function(){
  return this.key$getter().apply(this, arguments);
}
;
SplayTreeNode$Dart.prototype.key$getter = function(){
  return this.key$field;
}
;
SplayTreeNode$Dart.prototype.key$setter = function(tmp$0){
  this.key$field = tmp$0;
}
;
SplayTreeNode$Dart.prototype.value$named = function(){
  return this.value$getter().apply(this, arguments);
}
;
SplayTreeNode$Dart.prototype.value$getter = function(){
  return this.value$field;
}
;
SplayTreeNode$Dart.prototype.value$setter = function(tmp$0){
  this.value$field = tmp$0;
}
;
SplayTreeNode$Dart.prototype.left$named = function(){
  return this.left$getter().apply(this, arguments);
}
;
SplayTreeNode$Dart.prototype.left$getter = function(){
  return this.left$field;
}
;
SplayTreeNode$Dart.prototype.left$setter = function(tmp$0){
  this.left$field = tmp$0;
}
;
SplayTreeNode$Dart.prototype.right$named = function(){
  return this.right$getter().apply(this, arguments);
}
;
SplayTreeNode$Dart.prototype.right$getter = function(){
  return this.right$field;
}
;
SplayTreeNode$Dart.prototype.right$setter = function(tmp$0){
  this.right$field = tmp$0;
}
;
function SplayTree$Dart(){
}

SplayTree$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('SplayTree$Dart'), SplayTree$Dart.$RTTimplements, typeArgs);
}
;
SplayTree$Dart.$RTTimplements = function(rtt, typeArgs){
  SplayTree$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
SplayTree$Dart.$addTo = function(target, typeArgs){
  var rtt = SplayTree$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  Map$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0), RTT.getTypeArg(target.typeArgs, 1)]);
}
;
SplayTree$Dart.prototype.$implements$SplayTree$Dart = 1;
SplayTree$Dart.prototype.$implements$Map$Dart = 1;
SplayTree$Dart.prototype.$implements$Object$Dart = 1;
SplayTree$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
  var tmp$1, tmp$0;
  this._dummy$$setter_(tmp$0 = SplayTreeNode$Dart.SplayTreeNode$$Factory(SplayTreeNode$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('SplayTree$Dart')), 0), RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('SplayTree$Dart')), 1)]), $Dart$Null, $Dart$Null)) , tmp$0;
  this._count$$setter_(tmp$1 = 0) , tmp$1;
}
;
SplayTree$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
SplayTree$Dart.SplayTree$$Factory = function($rtt){
  var tmp$0 = new SplayTree$Dart;
  tmp$0.$typeInfo = $rtt;
  SplayTree$Dart.$Initializer.call(tmp$0);
  SplayTree$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
SplayTree$Dart.prototype._root$$named_ = function(){
  return this._root$$getter_().apply(this, arguments);
}
;
SplayTree$Dart.prototype._root$$getter_ = function(){
  return this._root$$field_;
}
;
SplayTree$Dart.prototype._root$$setter_ = function(tmp$0){
  this._root$$field_ = tmp$0;
}
;
SplayTree$Dart.prototype._dummy$$named_ = function(){
  return this._dummy$$getter_().apply(this, arguments);
}
;
SplayTree$Dart.prototype._dummy$$getter_ = function(){
  return this._dummy$$field_;
}
;
SplayTree$Dart.prototype._dummy$$setter_ = function(tmp$0){
  this._dummy$$field_ = tmp$0;
}
;
SplayTree$Dart.prototype._count$$named_ = function(){
  return this._count$$getter_().apply(this, arguments);
}
;
SplayTree$Dart.prototype._count$$getter_ = function(){
  return this._count$$field_;
}
;
SplayTree$Dart.prototype._count$$setter_ = function(tmp$0){
  this._count$$field_ = tmp$0;
}
;
SplayTree$Dart.prototype.splay_$member = function(key){
  var tmp$11, tmp$10, tmp$12, tmp$9, tmp$5, tmp$6, tmp$7, tmp$8, tmp$1, tmp$2, tmp$3, tmp$4, tmp$0;
  if (this.isEmpty$member()) {
    return;
  }
  var left = this._dummy$$getter_();
  var right = this._dummy$$getter_();
  var current = this._root$$getter_();
  while (true) {
    if (LT$operator(key.compareTo$named(1, $noargs, current.key$getter()), 0)) {
      if (current.left$getter() == null) {
        break;
      }
      if (LT$operator(key.compareTo$named(1, $noargs, current.left$getter().key$getter()), 0)) {
        var tmp = current.left$getter();
        current.left$setter(tmp$0 = tmp.right$getter()) , tmp$0;
        tmp.right$setter(tmp$1 = current) , tmp$1;
        current = tmp;
        if (current.left$getter() == null) {
          break;
        }
      }
      right.left$setter(tmp$2 = current) , tmp$2;
      right = current;
      current = current.left$getter();
    }
     else {
      if (GT$operator(key.compareTo$named(1, $noargs, current.key$getter()), 0)) {
        if (current.right$getter() == null) {
          break;
        }
        if (GT$operator(key.compareTo$named(1, $noargs, current.right$getter().key$getter()), 0)) {
          var tmp_0 = current.right$getter();
          current.right$setter(tmp$3 = tmp_0.left$getter()) , tmp$3;
          tmp_0.left$setter(tmp$4 = current) , tmp$4;
          current = tmp_0;
          if (current.right$getter() == null) {
            break;
          }
        }
        left.right$setter(tmp$5 = current) , tmp$5;
        left = current;
        current = current.right$getter();
      }
       else {
        break;
      }
    }
  }
  left.right$setter(tmp$6 = current.left$getter()) , tmp$6;
  right.left$setter(tmp$7 = current.right$getter()) , tmp$7;
  current.left$setter(tmp$8 = this._dummy$$getter_().right$getter()) , tmp$8;
  current.right$setter(tmp$9 = this._dummy$$getter_().left$getter()) , tmp$9;
  this._root$$setter_(tmp$10 = current) , tmp$10;
  this._dummy$$getter_().right$setter(tmp$11 = $Dart$Null) , tmp$11;
  this._dummy$$getter_().left$setter(tmp$12 = $Dart$Null) , tmp$12;
}
;
SplayTree$Dart.prototype.splay_$named = function($n, $o, key){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return SplayTree$Dart.prototype.splay_$member.call(this, key);
}
;
SplayTree$Dart.prototype.splay_$getter = function splay_$getter(){
  return $bind(SplayTree$Dart.prototype.splay_$named, this);
}
;
SplayTree$Dart.prototype.INDEX$operator = function(key){
  if (!this.isEmpty$member()) {
    this.splay_$member(key);
    if (EQ$operator(this._root$$getter_().key$getter().compareTo$named(1, $noargs, key), 0)) {
      return this._root$$getter_().value$getter();
    }
  }
  return $Dart$Null;
}
;
SplayTree$Dart.prototype.remove$member = function(key){
  var tmp$1, tmp$2, tmp$3, tmp$4, tmp$0;
  if (this.isEmpty$member()) {
    return $Dart$Null;
  }
  this.splay_$member(key);
  if (NE$operator(this._root$$getter_().key$getter().compareTo$named(1, $noargs, key), 0)) {
    return $Dart$Null;
  }
  var value = this._root$$getter_().value$getter();
  tmp$0 = this._count$$getter_() , (this._count$$setter_(tmp$1 = SUB$operator(tmp$0, 1)) , tmp$1 , tmp$0);
  if (this._root$$getter_().left$getter() == null) {
    this._root$$setter_(tmp$2 = this._root$$getter_().right$getter()) , tmp$2;
  }
   else {
    var right = this._root$$getter_().right$getter();
    this._root$$setter_(tmp$3 = this._root$$getter_().left$getter()) , tmp$3;
    this.splay_$member(key);
    this._root$$getter_().right$setter(tmp$4 = right) , tmp$4;
  }
  return value;
}
;
SplayTree$Dart.prototype.remove$named = function($n, $o, key){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return SplayTree$Dart.prototype.remove$member.call(this, key);
}
;
SplayTree$Dart.prototype.remove$getter = function remove$getter(){
  return $bind(SplayTree$Dart.prototype.remove$named, this);
}
;
SplayTree$Dart.prototype.ASSIGN_INDEX$operator = function(key, value){
  var tmp$11, tmp$10, tmp$12, tmp$9, tmp$5, tmp$6, tmp$7, tmp$8, tmp$1, tmp$2, tmp$3, tmp$4, tmp$0;
  if (this.isEmpty$member()) {
    tmp$0 = this._count$$getter_() , (this._count$$setter_(tmp$1 = ADD$operator(tmp$0, 1)) , tmp$1 , tmp$0);
    this._root$$setter_(tmp$2 = SplayTreeNode$Dart.SplayTreeNode$$Factory(SplayTreeNode$Dart.$lookupRTT(), key, value)) , tmp$2;
    return;
  }
  this.splay_$member(key);
  if (EQ$operator(this._root$$getter_().key$getter().compareTo$named(1, $noargs, key), 0)) {
    this._root$$getter_().value$setter(tmp$3 = value) , tmp$3;
    return;
  }
  var node = SplayTreeNode$Dart.SplayTreeNode$$Factory(SplayTreeNode$Dart.$lookupRTT(), key, value);
  tmp$4 = this._count$$getter_() , (this._count$$setter_(tmp$5 = ADD$operator(tmp$4, 1)) , tmp$5 , tmp$4);
  if (GT$operator(key.compareTo$named(1, $noargs, this._root$$getter_().key$getter()), 0)) {
    node.left$setter(tmp$6 = this._root$$getter_()) , tmp$6;
    node.right$setter(tmp$7 = this._root$$getter_().right$getter()) , tmp$7;
    this._root$$getter_().right$setter(tmp$8 = $Dart$Null) , tmp$8;
  }
   else {
    node.right$setter(tmp$9 = this._root$$getter_()) , tmp$9;
    node.left$setter(tmp$10 = this._root$$getter_().left$getter()) , tmp$10;
    this._root$$getter_().left$setter(tmp$11 = $Dart$Null) , tmp$11;
  }
  this._root$$setter_(tmp$12 = node) , tmp$12;
}
;
SplayTree$Dart.prototype.putIfAbsent$member = function(key, ifAbsent){
  var tmp$0;
  if (this.containsKey$member(key)) {
    return this.INDEX$operator(key);
  }
  var value = ifAbsent(0, $noargs);
  this.ASSIGN_INDEX$operator(key, tmp$0 = value) , tmp$0;
  return value;
}
;
SplayTree$Dart.prototype.putIfAbsent$named = function($n, $o, key, ifAbsent){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return SplayTree$Dart.prototype.putIfAbsent$member.call(this, key, ifAbsent);
}
;
SplayTree$Dart.prototype.putIfAbsent$getter = function putIfAbsent$getter(){
  return $bind(SplayTree$Dart.prototype.putIfAbsent$named, this);
}
;
SplayTree$Dart.prototype.isEmpty$member = function(){
  return this._root$$getter_() == null;
}
;
SplayTree$Dart.prototype.isEmpty$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return SplayTree$Dart.prototype.isEmpty$member.call(this);
}
;
SplayTree$Dart.prototype.isEmpty$getter = function isEmpty$getter(){
  return $bind(SplayTree$Dart.prototype.isEmpty$named, this);
}
;
SplayTree$Dart.prototype.forEach$member = function(f){
  var list = ListFactory$Dart.List$$Factory([SplayTreeNode$Dart.$lookupRTT([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('SplayTree$Dart')), 0), RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('SplayTree$Dart')), 1)])], $Dart$Null);
  var current = this._root$$getter_();
  while (NE$operator(current, $Dart$Null)) {
    if (NE$operator(current.left$getter(), $Dart$Null)) {
      list.add$named(1, $noargs, current);
      current = current.left$getter();
    }
     else {
      f(2, $noargs, current.key$getter(), current.value$getter());
      while (EQ$operator(current.right$getter(), $Dart$Null)) {
        if (list.isEmpty$named(0, $noargs)) {
          return;
        }
        current = list.removeLast$named(0, $noargs);
        f(2, $noargs, current.key$getter(), current.value$getter());
      }
      current = current.right$getter();
    }
  }
}
;
SplayTree$Dart.prototype.forEach$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return SplayTree$Dart.prototype.forEach$member.call(this, f);
}
;
SplayTree$Dart.prototype.forEach$getter = function forEach$getter(){
  return $bind(SplayTree$Dart.prototype.forEach$named, this);
}
;
SplayTree$Dart.prototype.length$named = function(){
  return this.length$getter().apply(this, arguments);
}
;
SplayTree$Dart.prototype.length$getter = function(){
  return this._count$$getter_();
}
;
SplayTree$Dart.prototype.clear$member = function(){
  var tmp$1, tmp$0;
  this._root$$setter_(tmp$0 = $Dart$Null) , tmp$0;
  this._count$$setter_(tmp$1 = 0) , tmp$1;
}
;
SplayTree$Dart.prototype.clear$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return SplayTree$Dart.prototype.clear$member.call(this);
}
;
SplayTree$Dart.prototype.clear$getter = function clear$getter(){
  return $bind(SplayTree$Dart.prototype.clear$named, this);
}
;
SplayTree$Dart.prototype.containsKey$member = function(key){
  if (!this.isEmpty$member()) {
    this.splay_$member(key);
    if (EQ$operator(this._root$$getter_().key$getter().compareTo$named(1, $noargs, key), 0)) {
      return true;
    }
  }
  return false;
}
;
SplayTree$Dart.prototype.containsKey$named = function($n, $o, key){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return SplayTree$Dart.prototype.containsKey$member.call(this, key);
}
;
SplayTree$Dart.prototype.containsKey$getter = function containsKey$getter(){
  return $bind(SplayTree$Dart.prototype.containsKey$named, this);
}
;
function SplayTree$Dart$containsValue$c0$14_14$Hoisted(dartc_scp$0, dartc_scp$1, k, v){
  if (EQ$operator(dartc_scp$0.value, v)) {
    dartc_scp$1.found = true;
  }
}

function SplayTree$Dart$containsValue$c0$14_14$Hoisted$named($s0, $s1, $n, $o, k, v){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return SplayTree$Dart$containsValue$c0$14_14$Hoisted($s0, $s1, k, v);
}

SplayTree$Dart.prototype.containsValue$member = function(value){
  var dartc_scp$0 = {value:value};
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.found = false;
  this.forEach$member($bind(SplayTree$Dart$containsValue$c0$14_14$Hoisted$named, $Dart$Null, dartc_scp$0, dartc_scp$1));
  return dartc_scp$1.found;
  dartc_scp$1 = $Dart$Null;
}
;
SplayTree$Dart.prototype.containsValue$named = function($n, $o, value){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return SplayTree$Dart.prototype.containsValue$member.call(this, value);
}
;
SplayTree$Dart.prototype.containsValue$getter = function containsValue$getter(){
  return $bind(SplayTree$Dart.prototype.containsValue$named, this);
}
;
function SplayTree$Dart$getKeys$c0$14_14$Hoisted(dartc_scp$1, k, v){
  dartc_scp$1.list.add$named(1, $noargs, k);
}

function SplayTree$Dart$getKeys$c0$14_14$Hoisted$named($s0, $n, $o, k, v){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return SplayTree$Dart$getKeys$c0$14_14$Hoisted($s0, k, v);
}

SplayTree$Dart.prototype.getKeys$member = function(){
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.list = ListFactory$Dart.List$$Factory([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('SplayTree$Dart')), 0)], $Dart$Null);
  this.forEach$member($bind(SplayTree$Dart$getKeys$c0$14_14$Hoisted$named, $Dart$Null, dartc_scp$1));
  return dartc_scp$1.list;
  dartc_scp$1 = $Dart$Null;
}
;
SplayTree$Dart.prototype.getKeys$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return SplayTree$Dart.prototype.getKeys$member.call(this);
}
;
SplayTree$Dart.prototype.getKeys$getter = function getKeys$getter(){
  return $bind(SplayTree$Dart.prototype.getKeys$named, this);
}
;
function SplayTree$Dart$getValues$c0$14_14$Hoisted(dartc_scp$1, k, v){
  dartc_scp$1.list.add$named(1, $noargs, v);
}

function SplayTree$Dart$getValues$c0$14_14$Hoisted$named($s0, $n, $o, k, v){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return SplayTree$Dart$getValues$c0$14_14$Hoisted($s0, k, v);
}

SplayTree$Dart.prototype.getValues$member = function(){
  var dartc_scp$1;
  dartc_scp$1 = {};
  dartc_scp$1.list = ListFactory$Dart.List$$Factory([RTT.getTypeArg(RTT.getTypeArgsFor(this, $cls('SplayTree$Dart')), 1)], $Dart$Null);
  this.forEach$member($bind(SplayTree$Dart$getValues$c0$14_14$Hoisted$named, $Dart$Null, dartc_scp$1));
  return dartc_scp$1.list;
  dartc_scp$1 = $Dart$Null;
}
;
SplayTree$Dart.prototype.getValues$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return SplayTree$Dart.prototype.getValues$member.call(this);
}
;
SplayTree$Dart.prototype.getValues$getter = function getValues$getter(){
  return $bind(SplayTree$Dart.prototype.getValues$named, this);
}
;
function StopWatchImplementation$Dart(){
}

StopWatchImplementation$Dart.$lookupRTT = function(){
  return RTT.create($cls('StopWatchImplementation$Dart'), StopWatchImplementation$Dart.$RTTimplements);
}
;
StopWatchImplementation$Dart.$RTTimplements = function(rtt){
  StopWatchImplementation$Dart.$addTo(rtt);
}
;
StopWatchImplementation$Dart.$addTo = function(target){
  var rtt = StopWatchImplementation$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  StopWatch$Dart.$addTo(target);
}
;
StopWatchImplementation$Dart.prototype.$implements$StopWatchImplementation$Dart = 1;
StopWatchImplementation$Dart.prototype.$implements$StopWatch$Dart = 1;
StopWatchImplementation$Dart.prototype.$implements$Object$Dart = 1;
StopWatchImplementation$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
StopWatchImplementation$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
  this._start$$field_ = $Dart$Null;
  this._stop$$field_ = $Dart$Null;
}
;
StopWatchImplementation$Dart.StopWatchImplementation$$Factory = function(){
  var tmp$0 = new StopWatchImplementation$Dart;
  tmp$0.$typeInfo = StopWatchImplementation$Dart.$lookupRTT();
  StopWatchImplementation$Dart.$Initializer.call(tmp$0);
  StopWatchImplementation$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
StopWatchImplementation$Dart.prototype._start$$named_ = function(){
  return this._start$$getter_().apply(this, arguments);
}
;
StopWatchImplementation$Dart.prototype._start$$getter_ = function(){
  return this._start$$field_;
}
;
StopWatchImplementation$Dart.prototype._start$$setter_ = function(tmp$0){
  this._start$$field_ = tmp$0;
}
;
StopWatchImplementation$Dart.prototype._stop$$named_ = function(){
  return this._stop$$getter_().apply(this, arguments);
}
;
StopWatchImplementation$Dart.prototype._stop$$getter_ = function(){
  return this._stop$$field_;
}
;
StopWatchImplementation$Dart.prototype._stop$$setter_ = function(tmp$0){
  this._stop$$field_ = tmp$0;
}
;
StopWatchImplementation$Dart.prototype.start$member = function(){
  var tmp$1, tmp$0;
  if (this._start$$getter_() == null) {
    this._start$$setter_(tmp$0 = Clock$Dart.now$member()) , tmp$0;
  }
   else {
    if (this._stop$$getter_() == null) {
      return;
    }
    this._start$$setter_(tmp$1 = SUB$operator(Clock$Dart.now$member(), SUB$operator(this._stop$$getter_(), this._start$$getter_()))) , tmp$1;
  }
}
;
StopWatchImplementation$Dart.prototype.start$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return StopWatchImplementation$Dart.prototype.start$member.call(this);
}
;
StopWatchImplementation$Dart.prototype.start$getter = function start$getter(){
  return $bind(StopWatchImplementation$Dart.prototype.start$named, this);
}
;
StopWatchImplementation$Dart.prototype.stop$member = function(){
  var tmp$0;
  if (this._start$$getter_() == null) {
    return;
  }
  this._stop$$setter_(tmp$0 = Clock$Dart.now$member()) , tmp$0;
}
;
StopWatchImplementation$Dart.prototype.stop$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return StopWatchImplementation$Dart.prototype.stop$member.call(this);
}
;
StopWatchImplementation$Dart.prototype.stop$getter = function stop$getter(){
  return $bind(StopWatchImplementation$Dart.prototype.stop$named, this);
}
;
StopWatchImplementation$Dart.prototype.elapsed$member = function(){
  if (this._start$$getter_() == null) {
    return 0;
  }
  return this._stop$$getter_() == null?SUB$operator(Clock$Dart.now$member(), this._start$$getter_()):SUB$operator(this._stop$$getter_(), this._start$$getter_());
}
;
StopWatchImplementation$Dart.prototype.elapsed$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return StopWatchImplementation$Dart.prototype.elapsed$member.call(this);
}
;
StopWatchImplementation$Dart.prototype.elapsed$getter = function elapsed$getter(){
  return $bind(StopWatchImplementation$Dart.prototype.elapsed$named, this);
}
;
StopWatchImplementation$Dart.prototype.elapsedInUs$member = function(){
  return TRUNC$operator(MUL$operator(this.elapsed$member(), 1000000), this.frequency$member());
}
;
StopWatchImplementation$Dart.prototype.elapsedInUs$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return StopWatchImplementation$Dart.prototype.elapsedInUs$member.call(this);
}
;
StopWatchImplementation$Dart.prototype.elapsedInUs$getter = function elapsedInUs$getter(){
  return $bind(StopWatchImplementation$Dart.prototype.elapsedInUs$named, this);
}
;
StopWatchImplementation$Dart.prototype.elapsedInMs$member = function(){
  return TRUNC$operator(MUL$operator(this.elapsed$member(), 1000), this.frequency$member());
}
;
StopWatchImplementation$Dart.prototype.elapsedInMs$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return StopWatchImplementation$Dart.prototype.elapsedInMs$member.call(this);
}
;
StopWatchImplementation$Dart.prototype.elapsedInMs$getter = function elapsedInMs$getter(){
  return $bind(StopWatchImplementation$Dart.prototype.elapsedInMs$named, this);
}
;
StopWatchImplementation$Dart.prototype.frequency$member = function(){
  return Clock$Dart.frequency$member();
}
;
StopWatchImplementation$Dart.prototype.frequency$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return StopWatchImplementation$Dart.prototype.frequency$member.call(this);
}
;
StopWatchImplementation$Dart.prototype.frequency$getter = function frequency$getter(){
  return $bind(StopWatchImplementation$Dart.prototype.frequency$named, this);
}
;
function Clock$Dart(){
}

Clock$Dart.$lookupRTT = function(){
  return RTT.create($cls('Clock$Dart'));
}
;
Clock$Dart.$addTo = function(target){
  var rtt = Clock$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
Clock$Dart.prototype.$implements$Clock$Dart = 1;
Clock$Dart.prototype.$implements$Object$Dart = 1;
Clock$Dart.now$member = function(){
  return DateImplementation$Dart.DateImplementation$now$18$Factory().value$getter();
}
;
Clock$Dart.now$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Clock$Dart.now$member();
}
;
Clock$Dart.now$getter = function now$getter(){
  return Clock$Dart.now$named;
}
;
Clock$Dart.frequency$member = function(){
  return 1000;
}
;
Clock$Dart.frequency$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Clock$Dart.frequency$member();
}
;
Clock$Dart.frequency$getter = function frequency$getter(){
  return Clock$Dart.frequency$named;
}
;
Clock$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
Clock$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
Clock$Dart.Clock$$Factory = function(){
  var tmp$0 = new Clock$Dart;
  tmp$0.$typeInfo = Clock$Dart.$lookupRTT();
  Clock$Dart.$Initializer.call(tmp$0);
  Clock$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function AssertionError$Dart(){
}

AssertionError$Dart.$lookupRTT = function(){
  return RTT.create($cls('AssertionError$Dart'));
}
;
AssertionError$Dart.$addTo = function(target){
  var rtt = AssertionError$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
AssertionError$Dart.prototype.$implements$AssertionError$Dart = 1;
AssertionError$Dart.prototype.$implements$Object$Dart = 1;
AssertionError$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
AssertionError$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
AssertionError$Dart.AssertionError$$Factory = function(){
  var tmp$0 = new AssertionError$Dart;
  tmp$0.$typeInfo = AssertionError$Dart.$lookupRTT();
  AssertionError$Dart.$Initializer.call(tmp$0);
  AssertionError$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
AssertionError$Dart.prototype.toString$member = function(){
  return 'Failed assertion';
}
;
AssertionError$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return AssertionError$Dart.prototype.toString$member.call(this);
}
;
AssertionError$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(AssertionError$Dart.prototype.toString$named, this);
}
;
AssertionError$Dart.prototype.$const_id = function(){
  return $cls('AssertionError$Dart');
}
;
function TypeError$Dart(){
}

TypeError$Dart.$lookupRTT = function(){
  return RTT.create($cls('TypeError$Dart'), TypeError$Dart.$RTTimplements);
}
;
TypeError$Dart.$RTTimplements = function(rtt){
  TypeError$Dart.$addTo(rtt);
}
;
TypeError$Dart.$addTo = function(target){
  var rtt = TypeError$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  AssertionError$Dart.$addTo(target);
}
;
TypeError$Dart.prototype.$implements$TypeError$Dart = 1;
TypeError$Dart.prototype.$implements$AssertionError$Dart = 1;
TypeError$Dart.prototype.$implements$Object$Dart = 1;
$inherits(TypeError$Dart, AssertionError$Dart);
TypeError$Dart.$Constructor = function(srcType, dstType){
  AssertionError$Dart.$Constructor.call(this);
}
;
TypeError$Dart.$Initializer = function(srcType, dstType){
  AssertionError$Dart.$Initializer.call(this);
  this.srcType$field = srcType;
  this.dstType$field = dstType;
}
;
TypeError$Dart.TypeError$$Factory = function(srcType, dstType){
  var tmp$0 = new TypeError$Dart;
  tmp$0.$typeInfo = TypeError$Dart.$lookupRTT();
  TypeError$Dart.$Initializer.call(tmp$0, srcType, dstType);
  TypeError$Dart.$Constructor.call(tmp$0, srcType, dstType);
  return tmp$0;
}
;
TypeError$Dart.prototype.dstType$named = function(){
  return this.dstType$getter().apply(this, arguments);
}
;
TypeError$Dart.prototype.dstType$getter = function(){
  return this.dstType$field;
}
;
TypeError$Dart.prototype.srcType$named = function(){
  return this.srcType$getter().apply(this, arguments);
}
;
TypeError$Dart.prototype.srcType$getter = function(){
  return this.srcType$field;
}
;
TypeError$Dart.prototype.toString$member = function(){
  return 'Failed type check: type ' + $toString(this.srcType$getter()) + ' is not assignable to type ' + $toString(this.dstType$getter()) + '';
}
;
TypeError$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return TypeError$Dart.prototype.toString$member.call(this);
}
;
TypeError$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(TypeError$Dart.prototype.toString$named, this);
}
;
TypeError$Dart.prototype.$const_id = function(){
  return $cls('TypeError$Dart') + (':' + $dart_const_id(this.dstType$field)) + (':' + $dart_const_id(this.srcType$field)) + ('-' + AssertionError$Dart.prototype.$const_id.call(this));
}
;
function FallThroughError$Dart(){
}

FallThroughError$Dart.$lookupRTT = function(){
  return RTT.create($cls('FallThroughError$Dart'));
}
;
FallThroughError$Dart.$addTo = function(target){
  var rtt = FallThroughError$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
FallThroughError$Dart.prototype.$implements$FallThroughError$Dart = 1;
FallThroughError$Dart.prototype.$implements$Object$Dart = 1;
FallThroughError$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
FallThroughError$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
FallThroughError$Dart.FallThroughError$$Factory = function(){
  var tmp$0 = new FallThroughError$Dart;
  tmp$0.$typeInfo = FallThroughError$Dart.$lookupRTT();
  FallThroughError$Dart.$Initializer.call(tmp$0);
  FallThroughError$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
FallThroughError$Dart.prototype.toString$member = function(){
  return 'Switch case fall-through';
}
;
FallThroughError$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return FallThroughError$Dart.prototype.toString$member.call(this);
}
;
FallThroughError$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(FallThroughError$Dart.prototype.toString$named, this);
}
;
FallThroughError$Dart.prototype.$const_id = function(){
  return $cls('FallThroughError$Dart');
}
;
Object.$lookupRTT = function(){
  return RTT.create($cls('Object'));
}
;
Object.$addTo = function(target){
  var rtt = Object.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
Object.prototype.$implements$Object$Dart = 1;
Object.$Constructor = function(){
}
;
Object.$Initializer = function(){
}
;
Object.Object$$Factory = function(){
  var tmp$0 = new Object;
  tmp$0.$typeInfo = Object.$lookupRTT();
  Object.$Constructor.call(tmp$0);
  return tmp$0;
}
;
Object.prototype.EQ$operator = function(other){
  return this === other;
}
;
Object.prototype.toString$member = function(){
  return 'Object';
}
;
Object.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Object.prototype.toString$member.call(this);
}
;
Object.prototype.toString$getter = function toString$getter(){
  return $bind(Object.prototype.toString$named, this);
}
;
Object.prototype.dynamic$named = function(){
  return this.dynamic$getter().apply(this, arguments);
}
;
Object.prototype.dynamic$getter = function(){
  return this;
}
;
Object.prototype.$const_id = function(){
  return $cls('Object') + (':' + $dart_const_id(this.dynamic$field));
}
;
function _Logger$Dart(){
}

_Logger$Dart.$lookupRTT = function(){
  return RTT.create($cls('_Logger$Dart'));
}
;
_Logger$Dart.$addTo = function(target){
  var rtt = _Logger$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
_Logger$Dart.prototype.$implements$_Logger$Dart = 1;
_Logger$Dart.prototype.$implements$Object$Dart = 1;
_Logger$Dart.print$member = function(arg_0){
  _Logger$Dart._printString$$member_(arg_0 == null?'null':arg_0.toString$named(0, $noargs));
}
;
_Logger$Dart.print$named = function($n, $o, arg){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _Logger$Dart.print$member(arg);
}
;
_Logger$Dart.print$getter = function print$getter(){
  return _Logger$Dart.print$named;
}
;
_Logger$Dart._printString$$member_ = function(str){
  return native__Logger__printString(str);
}
;
_Logger$Dart._printString$$named_ = function($n, $o, str){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return _Logger$Dart._printString$$member_(str);
}
;
_Logger$Dart._printString$$getter_ = function _printString$$getter_(){
  return _Logger$Dart._printString$$named_;
}
;
_Logger$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
_Logger$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
_Logger$Dart._Logger$$Factory = function(){
  var tmp$0 = new _Logger$Dart;
  tmp$0.$typeInfo = _Logger$Dart.$lookupRTT();
  _Logger$Dart.$Initializer.call(tmp$0);
  _Logger$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function print$getter(){
  return _Logger$Dart.print$named;
}

function print$named($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return print$getter();
}

function bool$Dart(){
}

bool$Dart.$lookupRTT = function(){
  return RTT.create($cls('bool$Dart'));
}
;
bool$Dart.$addTo = function(target){
  var rtt = bool$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
function Collection$Dart(){
}

Collection$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('Collection$Dart'), Collection$Dart.$RTTimplements, typeArgs);
}
;
Collection$Dart.$RTTimplements = function(rtt, typeArgs){
  Collection$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
Collection$Dart.$addTo = function(target, typeArgs){
  var rtt = Collection$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  Iterable$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0)]);
}
;
function Comparable$Dart(){
}

Comparable$Dart.$lookupRTT = function(){
  return RTT.create($cls('Comparable$Dart'));
}
;
Comparable$Dart.$addTo = function(target){
  var rtt = Comparable$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
function Date$Dart(){
}

Date$Dart.$lookupRTT = function(){
  return RTT.create($cls('Date$Dart'), Date$Dart.$RTTimplements);
}
;
Date$Dart.$RTTimplements = function(rtt){
  Date$Dart.$addTo(rtt);
}
;
Date$Dart.$addTo = function(target){
  var rtt = Date$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Comparable$Dart.$addTo(target);
}
;
Date$Dart.MON$named = function(){
  return Date$Dart.MON$getter().apply(this, arguments);
}
;
Date$Dart.MON$getter = function(){
  return 0;
}
;
Date$Dart.TUE$named = function(){
  return Date$Dart.TUE$getter().apply(this, arguments);
}
;
Date$Dart.TUE$getter = function(){
  return 1;
}
;
Date$Dart.WED$named = function(){
  return Date$Dart.WED$getter().apply(this, arguments);
}
;
Date$Dart.WED$getter = function(){
  return 2;
}
;
Date$Dart.THU$named = function(){
  return Date$Dart.THU$getter().apply(this, arguments);
}
;
Date$Dart.THU$getter = function(){
  return 3;
}
;
Date$Dart.FRI$named = function(){
  return Date$Dart.FRI$getter().apply(this, arguments);
}
;
Date$Dart.FRI$getter = function(){
  return 4;
}
;
Date$Dart.SAT$named = function(){
  return Date$Dart.SAT$getter().apply(this, arguments);
}
;
Date$Dart.SAT$getter = function(){
  return 5;
}
;
Date$Dart.SUN$named = function(){
  return Date$Dart.SUN$getter().apply(this, arguments);
}
;
Date$Dart.SUN$getter = function(){
  return 6;
}
;
Date$Dart.DAYS_IN_WEEK$named = function(){
  return Date$Dart.DAYS_IN_WEEK$getter().apply(this, arguments);
}
;
Date$Dart.DAYS_IN_WEEK$getter = function(){
  return 7;
}
;
Date$Dart.JAN$named = function(){
  return Date$Dart.JAN$getter().apply(this, arguments);
}
;
Date$Dart.JAN$getter = function(){
  return 1;
}
;
Date$Dart.FEB$named = function(){
  return Date$Dart.FEB$getter().apply(this, arguments);
}
;
Date$Dart.FEB$getter = function(){
  return 2;
}
;
Date$Dart.MAR$named = function(){
  return Date$Dart.MAR$getter().apply(this, arguments);
}
;
Date$Dart.MAR$getter = function(){
  return 3;
}
;
Date$Dart.APR$named = function(){
  return Date$Dart.APR$getter().apply(this, arguments);
}
;
Date$Dart.APR$getter = function(){
  return 4;
}
;
Date$Dart.MAY$named = function(){
  return Date$Dart.MAY$getter().apply(this, arguments);
}
;
Date$Dart.MAY$getter = function(){
  return 5;
}
;
Date$Dart.JUN$named = function(){
  return Date$Dart.JUN$getter().apply(this, arguments);
}
;
Date$Dart.JUN$getter = function(){
  return 6;
}
;
Date$Dart.JUL$named = function(){
  return Date$Dart.JUL$getter().apply(this, arguments);
}
;
Date$Dart.JUL$getter = function(){
  return 7;
}
;
Date$Dart.AUG$named = function(){
  return Date$Dart.AUG$getter().apply(this, arguments);
}
;
Date$Dart.AUG$getter = function(){
  return 8;
}
;
Date$Dart.SEP$named = function(){
  return Date$Dart.SEP$getter().apply(this, arguments);
}
;
Date$Dart.SEP$getter = function(){
  return 9;
}
;
Date$Dart.OCT$named = function(){
  return Date$Dart.OCT$getter().apply(this, arguments);
}
;
Date$Dart.OCT$getter = function(){
  return 10;
}
;
Date$Dart.NOV$named = function(){
  return Date$Dart.NOV$getter().apply(this, arguments);
}
;
Date$Dart.NOV$getter = function(){
  return 11;
}
;
Date$Dart.DEC$named = function(){
  return Date$Dart.DEC$getter().apply(this, arguments);
}
;
Date$Dart.DEC$getter = function(){
  return 12;
}
;
function double$Dart(){
}

double$Dart.$lookupRTT = function(){
  return RTT.create($cls('double$Dart'), double$Dart.$RTTimplements);
}
;
double$Dart.$RTTimplements = function(rtt){
  double$Dart.$addTo(rtt);
}
;
double$Dart.$addTo = function(target){
  var rtt = double$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  num$Dart.$addTo(target);
}
;
double$Dart.NAN$named = function(){
  return double$Dart.NAN$getter().apply(this, arguments);
}
;
double$Dart.NAN$getter = function(){
  var tmp$0 = isolate$current.double$DartNAN$field;
  var tmp$1 = static$initializing;
  if (tmp$0 === tmp$1)
    throw 'circular initialization';
  if (tmp$0 !== static$uninitialized)
    return tmp$0;
  isolate$current.double$DartNAN$field = tmp$1;
  var tmp$2 = DIV$operator(0, 0);
  isolate$current.double$DartNAN$field = tmp$2;
  return tmp$2;
}
;
double$Dart.INFINITY$named = function(){
  return double$Dart.INFINITY$getter().apply(this, arguments);
}
;
double$Dart.INFINITY$getter = function(){
  var tmp$0 = isolate$current.double$DartINFINITY$field;
  var tmp$1 = static$initializing;
  if (tmp$0 === tmp$1)
    throw 'circular initialization';
  if (tmp$0 !== static$uninitialized)
    return tmp$0;
  isolate$current.double$DartINFINITY$field = tmp$1;
  var tmp$2 = DIV$operator(1, 0);
  isolate$current.double$DartINFINITY$field = tmp$2;
  return tmp$2;
}
;
double$Dart.NEGATIVE_INFINITY$named = function(){
  return double$Dart.NEGATIVE_INFINITY$getter().apply(this, arguments);
}
;
double$Dart.NEGATIVE_INFINITY$getter = function(){
  var tmp$0 = isolate$current.double$DartNEGATIVE_INFINITY$field;
  var tmp$1 = static$initializing;
  if (tmp$0 === tmp$1)
    throw 'circular initialization';
  if (tmp$0 !== static$uninitialized)
    return tmp$0;
  isolate$current.double$DartNEGATIVE_INFINITY$field = tmp$1;
  var tmp$2 = negate$operator(double$Dart.INFINITY$getter());
  isolate$current.double$DartNEGATIVE_INFINITY$field = tmp$2;
  return tmp$2;
}
;
function Duration$Dart(){
}

Duration$Dart.$lookupRTT = function(){
  return RTT.create($cls('Duration$Dart'), Duration$Dart.$RTTimplements);
}
;
Duration$Dart.$RTTimplements = function(rtt){
  Duration$Dart.$addTo(rtt);
}
;
Duration$Dart.$addTo = function(target){
  var rtt = Duration$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Comparable$Dart.$addTo(target);
}
;
Duration$Dart.MILLISECONDS_PER_SECOND$named = function(){
  return Duration$Dart.MILLISECONDS_PER_SECOND$getter().apply(this, arguments);
}
;
Duration$Dart.MILLISECONDS_PER_SECOND$getter = function(){
  return 1000;
}
;
Duration$Dart.SECONDS_PER_MINUTE$named = function(){
  return Duration$Dart.SECONDS_PER_MINUTE$getter().apply(this, arguments);
}
;
Duration$Dart.SECONDS_PER_MINUTE$getter = function(){
  return 60;
}
;
Duration$Dart.MINUTES_PER_HOUR$named = function(){
  return Duration$Dart.MINUTES_PER_HOUR$getter().apply(this, arguments);
}
;
Duration$Dart.MINUTES_PER_HOUR$getter = function(){
  return 60;
}
;
Duration$Dart.HOURS_PER_DAY$named = function(){
  return Duration$Dart.HOURS_PER_DAY$getter().apply(this, arguments);
}
;
Duration$Dart.HOURS_PER_DAY$getter = function(){
  return 24;
}
;
Duration$Dart.MILLISECONDS_PER_MINUTE$named = function(){
  return Duration$Dart.MILLISECONDS_PER_MINUTE$getter().apply(this, arguments);
}
;
Duration$Dart.MILLISECONDS_PER_MINUTE$getter = function(){
  var tmp$0 = isolate$current.Duration$DartMILLISECONDS_PER_MINUTE$field;
  var tmp$1 = static$initializing;
  if (tmp$0 === tmp$1)
    throw 'circular initialization';
  if (tmp$0 !== static$uninitialized)
    return tmp$0;
  isolate$current.Duration$DartMILLISECONDS_PER_MINUTE$field = tmp$1;
  var tmp$2 = MUL$operator(Duration$Dart.MILLISECONDS_PER_SECOND$getter(), Duration$Dart.SECONDS_PER_MINUTE$getter());
  isolate$current.Duration$DartMILLISECONDS_PER_MINUTE$field = tmp$2;
  return tmp$2;
}
;
Duration$Dart.MILLISECONDS_PER_HOUR$named = function(){
  return Duration$Dart.MILLISECONDS_PER_HOUR$getter().apply(this, arguments);
}
;
Duration$Dart.MILLISECONDS_PER_HOUR$getter = function(){
  var tmp$0 = isolate$current.Duration$DartMILLISECONDS_PER_HOUR$field;
  var tmp$1 = static$initializing;
  if (tmp$0 === tmp$1)
    throw 'circular initialization';
  if (tmp$0 !== static$uninitialized)
    return tmp$0;
  isolate$current.Duration$DartMILLISECONDS_PER_HOUR$field = tmp$1;
  var tmp$2 = MUL$operator(Duration$Dart.MILLISECONDS_PER_MINUTE$getter(), Duration$Dart.MINUTES_PER_HOUR$getter());
  isolate$current.Duration$DartMILLISECONDS_PER_HOUR$field = tmp$2;
  return tmp$2;
}
;
Duration$Dart.MILLISECONDS_PER_DAY$named = function(){
  return Duration$Dart.MILLISECONDS_PER_DAY$getter().apply(this, arguments);
}
;
Duration$Dart.MILLISECONDS_PER_DAY$getter = function(){
  var tmp$0 = isolate$current.Duration$DartMILLISECONDS_PER_DAY$field;
  var tmp$1 = static$initializing;
  if (tmp$0 === tmp$1)
    throw 'circular initialization';
  if (tmp$0 !== static$uninitialized)
    return tmp$0;
  isolate$current.Duration$DartMILLISECONDS_PER_DAY$field = tmp$1;
  var tmp$2 = MUL$operator(Duration$Dart.MILLISECONDS_PER_HOUR$getter(), Duration$Dart.HOURS_PER_DAY$getter());
  isolate$current.Duration$DartMILLISECONDS_PER_DAY$field = tmp$2;
  return tmp$2;
}
;
Duration$Dart.SECONDS_PER_HOUR$named = function(){
  return Duration$Dart.SECONDS_PER_HOUR$getter().apply(this, arguments);
}
;
Duration$Dart.SECONDS_PER_HOUR$getter = function(){
  var tmp$0 = isolate$current.Duration$DartSECONDS_PER_HOUR$field;
  var tmp$1 = static$initializing;
  if (tmp$0 === tmp$1)
    throw 'circular initialization';
  if (tmp$0 !== static$uninitialized)
    return tmp$0;
  isolate$current.Duration$DartSECONDS_PER_HOUR$field = tmp$1;
  var tmp$2 = MUL$operator(Duration$Dart.SECONDS_PER_MINUTE$getter(), Duration$Dart.MINUTES_PER_HOUR$getter());
  isolate$current.Duration$DartSECONDS_PER_HOUR$field = tmp$2;
  return tmp$2;
}
;
Duration$Dart.SECONDS_PER_DAY$named = function(){
  return Duration$Dart.SECONDS_PER_DAY$getter().apply(this, arguments);
}
;
Duration$Dart.SECONDS_PER_DAY$getter = function(){
  var tmp$0 = isolate$current.Duration$DartSECONDS_PER_DAY$field;
  var tmp$1 = static$initializing;
  if (tmp$0 === tmp$1)
    throw 'circular initialization';
  if (tmp$0 !== static$uninitialized)
    return tmp$0;
  isolate$current.Duration$DartSECONDS_PER_DAY$field = tmp$1;
  var tmp$2 = MUL$operator(Duration$Dart.SECONDS_PER_HOUR$getter(), Duration$Dart.HOURS_PER_DAY$getter());
  isolate$current.Duration$DartSECONDS_PER_DAY$field = tmp$2;
  return tmp$2;
}
;
Duration$Dart.MINUTES_PER_DAY$named = function(){
  return Duration$Dart.MINUTES_PER_DAY$getter().apply(this, arguments);
}
;
Duration$Dart.MINUTES_PER_DAY$getter = function(){
  var tmp$0 = isolate$current.Duration$DartMINUTES_PER_DAY$field;
  var tmp$1 = static$initializing;
  if (tmp$0 === tmp$1)
    throw 'circular initialization';
  if (tmp$0 !== static$uninitialized)
    return tmp$0;
  isolate$current.Duration$DartMINUTES_PER_DAY$field = tmp$1;
  var tmp$2 = MUL$operator(Duration$Dart.MINUTES_PER_HOUR$getter(), Duration$Dart.HOURS_PER_DAY$getter());
  isolate$current.Duration$DartMINUTES_PER_DAY$field = tmp$2;
  return tmp$2;
}
;
function Exception$Dart(){
}

Exception$Dart.$lookupRTT = function(){
  return RTT.create($cls('Exception$Dart'));
}
;
Exception$Dart.$addTo = function(target){
  var rtt = Exception$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
function IndexOutOfRangeException$Dart(){
}

IndexOutOfRangeException$Dart.$lookupRTT = function(){
  return RTT.create($cls('IndexOutOfRangeException$Dart'), IndexOutOfRangeException$Dart.$RTTimplements);
}
;
IndexOutOfRangeException$Dart.$RTTimplements = function(rtt){
  IndexOutOfRangeException$Dart.$addTo(rtt);
}
;
IndexOutOfRangeException$Dart.$addTo = function(target){
  var rtt = IndexOutOfRangeException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
IndexOutOfRangeException$Dart.prototype.$implements$IndexOutOfRangeException$Dart = 1;
IndexOutOfRangeException$Dart.prototype.$implements$Exception$Dart = 1;
IndexOutOfRangeException$Dart.prototype.$implements$Object$Dart = 1;
IndexOutOfRangeException$Dart.$Constructor = function(_index){
  Object.$Constructor.call(this);
}
;
IndexOutOfRangeException$Dart.$Initializer = function(_index){
  Object.$Initializer.call(this);
  this._index$$field_ = _index;
}
;
IndexOutOfRangeException$Dart.IndexOutOfRangeException$$Factory = function(_index){
  var tmp$0 = new IndexOutOfRangeException$Dart;
  tmp$0.$typeInfo = IndexOutOfRangeException$Dart.$lookupRTT();
  IndexOutOfRangeException$Dart.$Initializer.call(tmp$0, _index);
  IndexOutOfRangeException$Dart.$Constructor.call(tmp$0, _index);
  return tmp$0;
}
;
IndexOutOfRangeException$Dart.prototype.toString$member = function(){
  return 'IndexOutOfRangeException: ' + $toString(this._index$$getter_()) + '';
}
;
IndexOutOfRangeException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return IndexOutOfRangeException$Dart.prototype.toString$member.call(this);
}
;
IndexOutOfRangeException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(IndexOutOfRangeException$Dart.prototype.toString$named, this);
}
;
IndexOutOfRangeException$Dart.prototype._index$$named_ = function(){
  return this._index$$getter_().apply(this, arguments);
}
;
IndexOutOfRangeException$Dart.prototype._index$$getter_ = function(){
  return this._index$$field_;
}
;
IndexOutOfRangeException$Dart.prototype.$const_id = function(){
  return $cls('IndexOutOfRangeException$Dart') + (':' + $dart_const_id(this._index$$field_));
}
;
function IllegalAccessException$Dart(){
}

IllegalAccessException$Dart.$lookupRTT = function(){
  return RTT.create($cls('IllegalAccessException$Dart'), IllegalAccessException$Dart.$RTTimplements);
}
;
IllegalAccessException$Dart.$RTTimplements = function(rtt){
  IllegalAccessException$Dart.$addTo(rtt);
}
;
IllegalAccessException$Dart.$addTo = function(target){
  var rtt = IllegalAccessException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
IllegalAccessException$Dart.prototype.$implements$IllegalAccessException$Dart = 1;
IllegalAccessException$Dart.prototype.$implements$Exception$Dart = 1;
IllegalAccessException$Dart.prototype.$implements$Object$Dart = 1;
IllegalAccessException$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
IllegalAccessException$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
IllegalAccessException$Dart.IllegalAccessException$$Factory = function(){
  var tmp$0 = new IllegalAccessException$Dart;
  tmp$0.$typeInfo = IllegalAccessException$Dart.$lookupRTT();
  IllegalAccessException$Dart.$Initializer.call(tmp$0);
  IllegalAccessException$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
IllegalAccessException$Dart.prototype.toString$member = function(){
  return 'Attempt to modify an immutable object';
}
;
IllegalAccessException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return IllegalAccessException$Dart.prototype.toString$member.call(this);
}
;
IllegalAccessException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(IllegalAccessException$Dart.prototype.toString$named, this);
}
;
IllegalAccessException$Dart.prototype.$const_id = function(){
  return $cls('IllegalAccessException$Dart');
}
;
function NoSuchMethodException$Dart(){
}

NoSuchMethodException$Dart.$lookupRTT = function(){
  return RTT.create($cls('NoSuchMethodException$Dart'), NoSuchMethodException$Dart.$RTTimplements);
}
;
NoSuchMethodException$Dart.$RTTimplements = function(rtt){
  NoSuchMethodException$Dart.$addTo(rtt);
}
;
NoSuchMethodException$Dart.$addTo = function(target){
  var rtt = NoSuchMethodException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
NoSuchMethodException$Dart.prototype.$implements$NoSuchMethodException$Dart = 1;
NoSuchMethodException$Dart.prototype.$implements$Exception$Dart = 1;
NoSuchMethodException$Dart.prototype.$implements$Object$Dart = 1;
NoSuchMethodException$Dart.$Constructor = function(_receiver, _functionName, _arguments){
  Object.$Constructor.call(this);
}
;
NoSuchMethodException$Dart.$Initializer = function(_receiver, _functionName, _arguments){
  Object.$Initializer.call(this);
  this._receiver$$field_ = _receiver;
  this._functionName$$field_ = _functionName;
  this._arguments$$field_ = _arguments;
}
;
NoSuchMethodException$Dart.NoSuchMethodException$$Factory = function(_receiver, _functionName, _arguments){
  var tmp$0 = new NoSuchMethodException$Dart;
  tmp$0.$typeInfo = NoSuchMethodException$Dart.$lookupRTT();
  NoSuchMethodException$Dart.$Initializer.call(tmp$0, _receiver, _functionName, _arguments);
  NoSuchMethodException$Dart.$Constructor.call(tmp$0, _receiver, _functionName, _arguments);
  return tmp$0;
}
;
NoSuchMethodException$Dart.prototype.toString$member = function(){
  var tmp$0;
  var sb = StringBufferImpl$Dart.StringBufferImpl$$Factory('');
  {
    var i = 0;
    for (; LT$operator(i, this._arguments$$getter_().length$getter()); tmp$0 = i , (i = ADD$operator(tmp$0, 1) , tmp$0)) {
      if (GT$operator(i, 0)) {
        sb.add$named(1, $noargs, ', ');
      }
      sb.add$named(1, $noargs, this._arguments$$getter_().INDEX$operator(i));
    }
  }
  sb.add$named(1, $noargs, ']');
  return ADD$operator("NoSuchMethodException - receiver: '" + $toString(this._receiver$$getter_()) + "' ", "function name: '" + $toString(this._functionName$$getter_()) + "' arguments: [" + $toString(sb) + ']');
}
;
NoSuchMethodException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return NoSuchMethodException$Dart.prototype.toString$member.call(this);
}
;
NoSuchMethodException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(NoSuchMethodException$Dart.prototype.toString$named, this);
}
;
NoSuchMethodException$Dart.prototype._receiver$$named_ = function(){
  return this._receiver$$getter_().apply(this, arguments);
}
;
NoSuchMethodException$Dart.prototype._receiver$$getter_ = function(){
  return this._receiver$$field_;
}
;
NoSuchMethodException$Dart.prototype._functionName$$named_ = function(){
  return this._functionName$$getter_().apply(this, arguments);
}
;
NoSuchMethodException$Dart.prototype._functionName$$getter_ = function(){
  return this._functionName$$field_;
}
;
NoSuchMethodException$Dart.prototype._arguments$$named_ = function(){
  return this._arguments$$getter_().apply(this, arguments);
}
;
NoSuchMethodException$Dart.prototype._arguments$$getter_ = function(){
  return this._arguments$$field_;
}
;
NoSuchMethodException$Dart.prototype.$const_id = function(){
  return $cls('NoSuchMethodException$Dart') + (':' + $dart_const_id(this._receiver$$field_)) + (':' + $dart_const_id(this._functionName$$field_)) + (':' + $dart_const_id(this._arguments$$field_));
}
;
function ClosureArgumentMismatchException$Dart(){
}

ClosureArgumentMismatchException$Dart.$lookupRTT = function(){
  return RTT.create($cls('ClosureArgumentMismatchException$Dart'), ClosureArgumentMismatchException$Dart.$RTTimplements);
}
;
ClosureArgumentMismatchException$Dart.$RTTimplements = function(rtt){
  ClosureArgumentMismatchException$Dart.$addTo(rtt);
}
;
ClosureArgumentMismatchException$Dart.$addTo = function(target){
  var rtt = ClosureArgumentMismatchException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
ClosureArgumentMismatchException$Dart.prototype.$implements$ClosureArgumentMismatchException$Dart = 1;
ClosureArgumentMismatchException$Dart.prototype.$implements$Exception$Dart = 1;
ClosureArgumentMismatchException$Dart.prototype.$implements$Object$Dart = 1;
ClosureArgumentMismatchException$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
ClosureArgumentMismatchException$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
ClosureArgumentMismatchException$Dart.ClosureArgumentMismatchException$$Factory = function(){
  var tmp$0 = new ClosureArgumentMismatchException$Dart;
  tmp$0.$typeInfo = ClosureArgumentMismatchException$Dart.$lookupRTT();
  ClosureArgumentMismatchException$Dart.$Initializer.call(tmp$0);
  ClosureArgumentMismatchException$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
ClosureArgumentMismatchException$Dart.prototype.toString$member = function(){
  return 'Closure argument mismatch';
}
;
ClosureArgumentMismatchException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return ClosureArgumentMismatchException$Dart.prototype.toString$member.call(this);
}
;
ClosureArgumentMismatchException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(ClosureArgumentMismatchException$Dart.prototype.toString$named, this);
}
;
ClosureArgumentMismatchException$Dart.prototype.$const_id = function(){
  return $cls('ClosureArgumentMismatchException$Dart');
}
;
function ObjectNotClosureException$Dart(){
}

ObjectNotClosureException$Dart.$lookupRTT = function(){
  return RTT.create($cls('ObjectNotClosureException$Dart'), ObjectNotClosureException$Dart.$RTTimplements);
}
;
ObjectNotClosureException$Dart.$RTTimplements = function(rtt){
  ObjectNotClosureException$Dart.$addTo(rtt);
}
;
ObjectNotClosureException$Dart.$addTo = function(target){
  var rtt = ObjectNotClosureException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
ObjectNotClosureException$Dart.prototype.$implements$ObjectNotClosureException$Dart = 1;
ObjectNotClosureException$Dart.prototype.$implements$Exception$Dart = 1;
ObjectNotClosureException$Dart.prototype.$implements$Object$Dart = 1;
ObjectNotClosureException$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
ObjectNotClosureException$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
ObjectNotClosureException$Dart.ObjectNotClosureException$$Factory = function(){
  var tmp$0 = new ObjectNotClosureException$Dart;
  tmp$0.$typeInfo = ObjectNotClosureException$Dart.$lookupRTT();
  ObjectNotClosureException$Dart.$Initializer.call(tmp$0);
  ObjectNotClosureException$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
ObjectNotClosureException$Dart.prototype.toString$member = function(){
  return 'Object is not closure';
}
;
ObjectNotClosureException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return ObjectNotClosureException$Dart.prototype.toString$member.call(this);
}
;
ObjectNotClosureException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(ObjectNotClosureException$Dart.prototype.toString$named, this);
}
;
ObjectNotClosureException$Dart.prototype.$const_id = function(){
  return $cls('ObjectNotClosureException$Dart');
}
;
function IllegalArgumentException$Dart(){
}

IllegalArgumentException$Dart.$lookupRTT = function(){
  return RTT.create($cls('IllegalArgumentException$Dart'), IllegalArgumentException$Dart.$RTTimplements);
}
;
IllegalArgumentException$Dart.$RTTimplements = function(rtt){
  IllegalArgumentException$Dart.$addTo(rtt);
}
;
IllegalArgumentException$Dart.$addTo = function(target){
  var rtt = IllegalArgumentException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
IllegalArgumentException$Dart.prototype.$implements$IllegalArgumentException$Dart = 1;
IllegalArgumentException$Dart.prototype.$implements$Exception$Dart = 1;
IllegalArgumentException$Dart.prototype.$implements$Object$Dart = 1;
IllegalArgumentException$Dart.$Constructor = function(_args){
  Object.$Constructor.call(this);
}
;
IllegalArgumentException$Dart.$Initializer = function(_args){
  Object.$Initializer.call(this);
  this._args$$field_ = _args;
}
;
IllegalArgumentException$Dart.IllegalArgumentException$$Factory = function(_args){
  var tmp$0 = new IllegalArgumentException$Dart;
  tmp$0.$typeInfo = IllegalArgumentException$Dart.$lookupRTT();
  IllegalArgumentException$Dart.$Initializer.call(tmp$0, _args);
  IllegalArgumentException$Dart.$Constructor.call(tmp$0, _args);
  return tmp$0;
}
;
IllegalArgumentException$Dart.prototype.toString$member = function(){
  return 'Illegal argument(s): ' + $toString(this._args$$getter_()) + '';
}
;
IllegalArgumentException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return IllegalArgumentException$Dart.prototype.toString$member.call(this);
}
;
IllegalArgumentException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(IllegalArgumentException$Dart.prototype.toString$named, this);
}
;
IllegalArgumentException$Dart.prototype._args$$named_ = function(){
  return this._args$$getter_().apply(this, arguments);
}
;
IllegalArgumentException$Dart.prototype._args$$getter_ = function(){
  return this._args$$field_;
}
;
IllegalArgumentException$Dart.prototype.$const_id = function(){
  return $cls('IllegalArgumentException$Dart') + (':' + $dart_const_id(this._args$$field_));
}
;
function OutOfMemoryException$Dart(){
}

OutOfMemoryException$Dart.$lookupRTT = function(){
  return RTT.create($cls('OutOfMemoryException$Dart'), OutOfMemoryException$Dart.$RTTimplements);
}
;
OutOfMemoryException$Dart.$RTTimplements = function(rtt){
  OutOfMemoryException$Dart.$addTo(rtt);
}
;
OutOfMemoryException$Dart.$addTo = function(target){
  var rtt = OutOfMemoryException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
OutOfMemoryException$Dart.prototype.$implements$OutOfMemoryException$Dart = 1;
OutOfMemoryException$Dart.prototype.$implements$Exception$Dart = 1;
OutOfMemoryException$Dart.prototype.$implements$Object$Dart = 1;
OutOfMemoryException$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
OutOfMemoryException$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
OutOfMemoryException$Dart.OutOfMemoryException$$Factory = function(){
  var tmp$0 = new OutOfMemoryException$Dart;
  tmp$0.$typeInfo = OutOfMemoryException$Dart.$lookupRTT();
  OutOfMemoryException$Dart.$Initializer.call(tmp$0);
  OutOfMemoryException$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
OutOfMemoryException$Dart.prototype.toString$member = function(){
  return 'Out of Memory';
}
;
OutOfMemoryException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return OutOfMemoryException$Dart.prototype.toString$member.call(this);
}
;
OutOfMemoryException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(OutOfMemoryException$Dart.prototype.toString$named, this);
}
;
OutOfMemoryException$Dart.prototype.$const_id = function(){
  return $cls('OutOfMemoryException$Dart');
}
;
function StackOverflowException$Dart(){
}

StackOverflowException$Dart.$lookupRTT = function(){
  return RTT.create($cls('StackOverflowException$Dart'), StackOverflowException$Dart.$RTTimplements);
}
;
StackOverflowException$Dart.$RTTimplements = function(rtt){
  StackOverflowException$Dart.$addTo(rtt);
}
;
StackOverflowException$Dart.$addTo = function(target){
  var rtt = StackOverflowException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
StackOverflowException$Dart.prototype.$implements$StackOverflowException$Dart = 1;
StackOverflowException$Dart.prototype.$implements$Exception$Dart = 1;
StackOverflowException$Dart.prototype.$implements$Object$Dart = 1;
StackOverflowException$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
StackOverflowException$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
StackOverflowException$Dart.StackOverflowException$$Factory = function(){
  var tmp$0 = new StackOverflowException$Dart;
  tmp$0.$typeInfo = StackOverflowException$Dart.$lookupRTT();
  StackOverflowException$Dart.$Initializer.call(tmp$0);
  StackOverflowException$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
StackOverflowException$Dart.prototype.toString$member = function(){
  return 'Stack Overflow';
}
;
StackOverflowException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return StackOverflowException$Dart.prototype.toString$member.call(this);
}
;
StackOverflowException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(StackOverflowException$Dart.prototype.toString$named, this);
}
;
StackOverflowException$Dart.prototype.$const_id = function(){
  return $cls('StackOverflowException$Dart');
}
;
function BadNumberFormatException$Dart(){
}

BadNumberFormatException$Dart.$lookupRTT = function(){
  return RTT.create($cls('BadNumberFormatException$Dart'), BadNumberFormatException$Dart.$RTTimplements);
}
;
BadNumberFormatException$Dart.$RTTimplements = function(rtt){
  BadNumberFormatException$Dart.$addTo(rtt);
}
;
BadNumberFormatException$Dart.$addTo = function(target){
  var rtt = BadNumberFormatException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
BadNumberFormatException$Dart.prototype.$implements$BadNumberFormatException$Dart = 1;
BadNumberFormatException$Dart.prototype.$implements$Exception$Dart = 1;
BadNumberFormatException$Dart.prototype.$implements$Object$Dart = 1;
BadNumberFormatException$Dart.$Constructor = function(_s){
  Object.$Constructor.call(this);
}
;
BadNumberFormatException$Dart.$Initializer = function(_s){
  Object.$Initializer.call(this);
  this._s$$field_ = _s;
}
;
BadNumberFormatException$Dart.BadNumberFormatException$$Factory = function(_s){
  var tmp$0 = new BadNumberFormatException$Dart;
  tmp$0.$typeInfo = BadNumberFormatException$Dart.$lookupRTT();
  BadNumberFormatException$Dart.$Initializer.call(tmp$0, _s);
  BadNumberFormatException$Dart.$Constructor.call(tmp$0, _s);
  return tmp$0;
}
;
BadNumberFormatException$Dart.prototype.toString$member = function(){
  return "BadNumberFormatException: '" + $toString(this._s$$getter_()) + "'";
}
;
BadNumberFormatException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return BadNumberFormatException$Dart.prototype.toString$member.call(this);
}
;
BadNumberFormatException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(BadNumberFormatException$Dart.prototype.toString$named, this);
}
;
BadNumberFormatException$Dart.prototype._s$$named_ = function(){
  return this._s$$getter_().apply(this, arguments);
}
;
BadNumberFormatException$Dart.prototype._s$$getter_ = function(){
  return this._s$$field_;
}
;
BadNumberFormatException$Dart.prototype.$const_id = function(){
  return $cls('BadNumberFormatException$Dart') + (':' + $dart_const_id(this._s$$field_));
}
;
function WrongArgumentCountException$Dart(){
}

WrongArgumentCountException$Dart.$lookupRTT = function(){
  return RTT.create($cls('WrongArgumentCountException$Dart'), WrongArgumentCountException$Dart.$RTTimplements);
}
;
WrongArgumentCountException$Dart.$RTTimplements = function(rtt){
  WrongArgumentCountException$Dart.$addTo(rtt);
}
;
WrongArgumentCountException$Dart.$addTo = function(target){
  var rtt = WrongArgumentCountException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
WrongArgumentCountException$Dart.prototype.$implements$WrongArgumentCountException$Dart = 1;
WrongArgumentCountException$Dart.prototype.$implements$Exception$Dart = 1;
WrongArgumentCountException$Dart.prototype.$implements$Object$Dart = 1;
WrongArgumentCountException$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
WrongArgumentCountException$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
WrongArgumentCountException$Dart.WrongArgumentCountException$$Factory = function(){
  var tmp$0 = new WrongArgumentCountException$Dart;
  tmp$0.$typeInfo = WrongArgumentCountException$Dart.$lookupRTT();
  WrongArgumentCountException$Dart.$Initializer.call(tmp$0);
  WrongArgumentCountException$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
WrongArgumentCountException$Dart.prototype.toString$member = function(){
  return 'WrongArgumentCountException';
}
;
WrongArgumentCountException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return WrongArgumentCountException$Dart.prototype.toString$member.call(this);
}
;
WrongArgumentCountException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(WrongArgumentCountException$Dart.prototype.toString$named, this);
}
;
WrongArgumentCountException$Dart.prototype.$const_id = function(){
  return $cls('WrongArgumentCountException$Dart');
}
;
function NullPointerException$Dart(){
}

NullPointerException$Dart.$lookupRTT = function(){
  return RTT.create($cls('NullPointerException$Dart'), NullPointerException$Dart.$RTTimplements);
}
;
NullPointerException$Dart.$RTTimplements = function(rtt){
  NullPointerException$Dart.$addTo(rtt);
}
;
NullPointerException$Dart.$addTo = function(target){
  var rtt = NullPointerException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
NullPointerException$Dart.prototype.$implements$NullPointerException$Dart = 1;
NullPointerException$Dart.prototype.$implements$Exception$Dart = 1;
NullPointerException$Dart.prototype.$implements$Object$Dart = 1;
NullPointerException$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
NullPointerException$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
NullPointerException$Dart.NullPointerException$$Factory = function(){
  var tmp$0 = new NullPointerException$Dart;
  tmp$0.$typeInfo = NullPointerException$Dart.$lookupRTT();
  NullPointerException$Dart.$Initializer.call(tmp$0);
  NullPointerException$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
NullPointerException$Dart.prototype.toString$member = function(){
  return 'NullPointerException';
}
;
NullPointerException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return NullPointerException$Dart.prototype.toString$member.call(this);
}
;
NullPointerException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(NullPointerException$Dart.prototype.toString$named, this);
}
;
NullPointerException$Dart.prototype.$const_id = function(){
  return $cls('NullPointerException$Dart');
}
;
function NoMoreElementsException$Dart(){
}

NoMoreElementsException$Dart.$lookupRTT = function(){
  return RTT.create($cls('NoMoreElementsException$Dart'), NoMoreElementsException$Dart.$RTTimplements);
}
;
NoMoreElementsException$Dart.$RTTimplements = function(rtt){
  NoMoreElementsException$Dart.$addTo(rtt);
}
;
NoMoreElementsException$Dart.$addTo = function(target){
  var rtt = NoMoreElementsException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
NoMoreElementsException$Dart.prototype.$implements$NoMoreElementsException$Dart = 1;
NoMoreElementsException$Dart.prototype.$implements$Exception$Dart = 1;
NoMoreElementsException$Dart.prototype.$implements$Object$Dart = 1;
NoMoreElementsException$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
NoMoreElementsException$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
NoMoreElementsException$Dart.NoMoreElementsException$$Factory = function(){
  var tmp$0 = new NoMoreElementsException$Dart;
  tmp$0.$typeInfo = NoMoreElementsException$Dart.$lookupRTT();
  NoMoreElementsException$Dart.$Initializer.call(tmp$0);
  NoMoreElementsException$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
NoMoreElementsException$Dart.prototype.toString$member = function(){
  return 'NoMoreElementsException';
}
;
NoMoreElementsException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return NoMoreElementsException$Dart.prototype.toString$member.call(this);
}
;
NoMoreElementsException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(NoMoreElementsException$Dart.prototype.toString$named, this);
}
;
NoMoreElementsException$Dart.prototype.$const_id = function(){
  return $cls('NoMoreElementsException$Dart');
}
;
function EmptyQueueException$Dart(){
}

EmptyQueueException$Dart.$lookupRTT = function(){
  return RTT.create($cls('EmptyQueueException$Dart'), EmptyQueueException$Dart.$RTTimplements);
}
;
EmptyQueueException$Dart.$RTTimplements = function(rtt){
  EmptyQueueException$Dart.$addTo(rtt);
}
;
EmptyQueueException$Dart.$addTo = function(target){
  var rtt = EmptyQueueException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
EmptyQueueException$Dart.prototype.$implements$EmptyQueueException$Dart = 1;
EmptyQueueException$Dart.prototype.$implements$Exception$Dart = 1;
EmptyQueueException$Dart.prototype.$implements$Object$Dart = 1;
EmptyQueueException$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
EmptyQueueException$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
EmptyQueueException$Dart.EmptyQueueException$$Factory = function(){
  var tmp$0 = new EmptyQueueException$Dart;
  tmp$0.$typeInfo = EmptyQueueException$Dart.$lookupRTT();
  EmptyQueueException$Dart.$Initializer.call(tmp$0);
  EmptyQueueException$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
EmptyQueueException$Dart.prototype.toString$member = function(){
  return 'EmptyQueueException';
}
;
EmptyQueueException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return EmptyQueueException$Dart.prototype.toString$member.call(this);
}
;
EmptyQueueException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(EmptyQueueException$Dart.prototype.toString$named, this);
}
;
EmptyQueueException$Dart.prototype.$const_id = function(){
  return $cls('EmptyQueueException$Dart');
}
;
function UnsupportedOperationException$Dart(){
}

UnsupportedOperationException$Dart.$lookupRTT = function(){
  return RTT.create($cls('UnsupportedOperationException$Dart'), UnsupportedOperationException$Dart.$RTTimplements);
}
;
UnsupportedOperationException$Dart.$RTTimplements = function(rtt){
  UnsupportedOperationException$Dart.$addTo(rtt);
}
;
UnsupportedOperationException$Dart.$addTo = function(target){
  var rtt = UnsupportedOperationException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
UnsupportedOperationException$Dart.prototype.$implements$UnsupportedOperationException$Dart = 1;
UnsupportedOperationException$Dart.prototype.$implements$Exception$Dart = 1;
UnsupportedOperationException$Dart.prototype.$implements$Object$Dart = 1;
UnsupportedOperationException$Dart.$Constructor = function(_message){
  Object.$Constructor.call(this);
}
;
UnsupportedOperationException$Dart.$Initializer = function(_message){
  Object.$Initializer.call(this);
  this._message$$field_ = _message;
}
;
UnsupportedOperationException$Dart.UnsupportedOperationException$$Factory = function(_message){
  var tmp$0 = new UnsupportedOperationException$Dart;
  tmp$0.$typeInfo = UnsupportedOperationException$Dart.$lookupRTT();
  UnsupportedOperationException$Dart.$Initializer.call(tmp$0, _message);
  UnsupportedOperationException$Dart.$Constructor.call(tmp$0, _message);
  return tmp$0;
}
;
UnsupportedOperationException$Dart.prototype.toString$member = function(){
  return 'UnsupportedOperationException: ' + $toString(this._message$$getter_()) + '';
}
;
UnsupportedOperationException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return UnsupportedOperationException$Dart.prototype.toString$member.call(this);
}
;
UnsupportedOperationException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(UnsupportedOperationException$Dart.prototype.toString$named, this);
}
;
UnsupportedOperationException$Dart.prototype._message$$named_ = function(){
  return this._message$$getter_().apply(this, arguments);
}
;
UnsupportedOperationException$Dart.prototype._message$$getter_ = function(){
  return this._message$$field_;
}
;
UnsupportedOperationException$Dart.prototype.$const_id = function(){
  return $cls('UnsupportedOperationException$Dart') + (':' + $dart_const_id(this._message$$field_));
}
;
function NotImplementedException$Dart(){
}

NotImplementedException$Dart.$lookupRTT = function(){
  return RTT.create($cls('NotImplementedException$Dart'), NotImplementedException$Dart.$RTTimplements);
}
;
NotImplementedException$Dart.$RTTimplements = function(rtt){
  NotImplementedException$Dart.$addTo(rtt);
}
;
NotImplementedException$Dart.$addTo = function(target){
  var rtt = NotImplementedException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
NotImplementedException$Dart.prototype.$implements$NotImplementedException$Dart = 1;
NotImplementedException$Dart.prototype.$implements$Exception$Dart = 1;
NotImplementedException$Dart.prototype.$implements$Object$Dart = 1;
NotImplementedException$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
NotImplementedException$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
NotImplementedException$Dart.NotImplementedException$$Factory = function(){
  var tmp$0 = new NotImplementedException$Dart;
  tmp$0.$typeInfo = NotImplementedException$Dart.$lookupRTT();
  NotImplementedException$Dart.$Initializer.call(tmp$0);
  NotImplementedException$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
NotImplementedException$Dart.prototype.toString$member = function(){
  return 'NotImplementedException';
}
;
NotImplementedException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return NotImplementedException$Dart.prototype.toString$member.call(this);
}
;
NotImplementedException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(NotImplementedException$Dart.prototype.toString$named, this);
}
;
NotImplementedException$Dart.prototype.$const_id = function(){
  return $cls('NotImplementedException$Dart');
}
;
function IllegalJSRegExpException$Dart(){
}

IllegalJSRegExpException$Dart.$lookupRTT = function(){
  return RTT.create($cls('IllegalJSRegExpException$Dart'), IllegalJSRegExpException$Dart.$RTTimplements);
}
;
IllegalJSRegExpException$Dart.$RTTimplements = function(rtt){
  IllegalJSRegExpException$Dart.$addTo(rtt);
}
;
IllegalJSRegExpException$Dart.$addTo = function(target){
  var rtt = IllegalJSRegExpException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
IllegalJSRegExpException$Dart.prototype.$implements$IllegalJSRegExpException$Dart = 1;
IllegalJSRegExpException$Dart.prototype.$implements$Exception$Dart = 1;
IllegalJSRegExpException$Dart.prototype.$implements$Object$Dart = 1;
IllegalJSRegExpException$Dart.$Constructor = function(_pattern, _errmsg){
  Object.$Constructor.call(this);
}
;
IllegalJSRegExpException$Dart.$Initializer = function(_pattern, _errmsg){
  Object.$Initializer.call(this);
  this._pattern$$field_ = _pattern;
  this._errmsg$$field_ = _errmsg;
}
;
IllegalJSRegExpException$Dart.IllegalJSRegExpException$$Factory = function(_pattern, _errmsg){
  var tmp$0 = new IllegalJSRegExpException$Dart;
  tmp$0.$typeInfo = IllegalJSRegExpException$Dart.$lookupRTT();
  IllegalJSRegExpException$Dart.$Initializer.call(tmp$0, _pattern, _errmsg);
  IllegalJSRegExpException$Dart.$Constructor.call(tmp$0, _pattern, _errmsg);
  return tmp$0;
}
;
IllegalJSRegExpException$Dart.prototype.toString$member = function(){
  return "IllegalJSRegExpException: '" + $toString(this._pattern$$getter_()) + "' '" + $toString(this._errmsg$$getter_()) + "'";
}
;
IllegalJSRegExpException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return IllegalJSRegExpException$Dart.prototype.toString$member.call(this);
}
;
IllegalJSRegExpException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(IllegalJSRegExpException$Dart.prototype.toString$named, this);
}
;
IllegalJSRegExpException$Dart.prototype._pattern$$named_ = function(){
  return this._pattern$$getter_().apply(this, arguments);
}
;
IllegalJSRegExpException$Dart.prototype._pattern$$getter_ = function(){
  return this._pattern$$field_;
}
;
IllegalJSRegExpException$Dart.prototype._errmsg$$named_ = function(){
  return this._errmsg$$getter_().apply(this, arguments);
}
;
IllegalJSRegExpException$Dart.prototype._errmsg$$getter_ = function(){
  return this._errmsg$$field_;
}
;
IllegalJSRegExpException$Dart.prototype.$const_id = function(){
  return $cls('IllegalJSRegExpException$Dart') + (':' + $dart_const_id(this._pattern$$field_)) + (':' + $dart_const_id(this._errmsg$$field_));
}
;
function IntegerDivisionByZeroException$Dart(){
}

IntegerDivisionByZeroException$Dart.$lookupRTT = function(){
  return RTT.create($cls('IntegerDivisionByZeroException$Dart'), IntegerDivisionByZeroException$Dart.$RTTimplements);
}
;
IntegerDivisionByZeroException$Dart.$RTTimplements = function(rtt){
  IntegerDivisionByZeroException$Dart.$addTo(rtt);
}
;
IntegerDivisionByZeroException$Dart.$addTo = function(target){
  var rtt = IntegerDivisionByZeroException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
IntegerDivisionByZeroException$Dart.prototype.$implements$IntegerDivisionByZeroException$Dart = 1;
IntegerDivisionByZeroException$Dart.prototype.$implements$Exception$Dart = 1;
IntegerDivisionByZeroException$Dart.prototype.$implements$Object$Dart = 1;
IntegerDivisionByZeroException$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
IntegerDivisionByZeroException$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
IntegerDivisionByZeroException$Dart.IntegerDivisionByZeroException$$Factory = function(){
  var tmp$0 = new IntegerDivisionByZeroException$Dart;
  tmp$0.$typeInfo = IntegerDivisionByZeroException$Dart.$lookupRTT();
  IntegerDivisionByZeroException$Dart.$Initializer.call(tmp$0);
  IntegerDivisionByZeroException$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
IntegerDivisionByZeroException$Dart.prototype.toString$member = function(){
  return 'IntegerDivisionByZeroException';
}
;
IntegerDivisionByZeroException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return IntegerDivisionByZeroException$Dart.prototype.toString$member.call(this);
}
;
IntegerDivisionByZeroException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(IntegerDivisionByZeroException$Dart.prototype.toString$named, this);
}
;
IntegerDivisionByZeroException$Dart.prototype.$const_id = function(){
  return $cls('IntegerDivisionByZeroException$Dart');
}
;
function Expect$Dart(){
}

Expect$Dart.$lookupRTT = function(){
  return RTT.create($cls('Expect$Dart'));
}
;
Expect$Dart.$addTo = function(target){
  var rtt = Expect$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
Expect$Dart.prototype.$implements$Expect$Dart = 1;
Expect$Dart.prototype.$implements$Object$Dart = 1;
Expect$Dart.equals$member = function(expected, actual, reason){
  if (EQ$operator(expected, actual)) {
    return;
  }
  var msg = Expect$Dart._getMessage$$member_(reason);
  Expect$Dart._fail$$member_('Expect.equals(expected: <' + $toString(expected) + '>, actual: <' + $toString(actual) + '>' + $toString(msg) + ') fails.');
}
;
Expect$Dart.equals$named = function($n, $o, expected, actual, reason){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 2:
      reason = $o.reason?(++seen , $o.reason):(++def , $Dart$Null);
  }
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return Expect$Dart.equals$member(expected, actual, reason);
}
;
Expect$Dart.equals$getter = function equals$getter(){
  return Expect$Dart.equals$named;
}
;
Expect$Dart.isTrue$member = function(actual, reason){
  if (actual === true) {
    return;
  }
  var msg = Expect$Dart._getMessage$$member_(reason);
  Expect$Dart._fail$$member_('Expect.isTrue(' + $toString(actual) + '' + $toString(msg) + ') fails.');
}
;
Expect$Dart.isTrue$named = function($n, $o, actual, reason){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 1:
      reason = $o.reason?(++seen , $o.reason):(++def , $Dart$Null);
  }
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Expect$Dart.isTrue$member(actual, reason);
}
;
Expect$Dart.isTrue$getter = function isTrue$getter(){
  return Expect$Dart.isTrue$named;
}
;
Expect$Dart.isFalse$member = function(actual, reason){
  if (actual === false) {
    return;
  }
  var msg = Expect$Dart._getMessage$$member_(reason);
  Expect$Dart._fail$$member_('Expect.isFalse(' + $toString(actual) + '' + $toString(msg) + ') fails.');
}
;
Expect$Dart.isFalse$named = function($n, $o, actual, reason){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 1:
      reason = $o.reason?(++seen , $o.reason):(++def , $Dart$Null);
  }
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Expect$Dart.isFalse$member(actual, reason);
}
;
Expect$Dart.isFalse$getter = function isFalse$getter(){
  return Expect$Dart.isFalse$named;
}
;
Expect$Dart.isNull$member = function(actual, reason){
  if (actual == null) {
    return;
  }
  var msg = Expect$Dart._getMessage$$member_(reason);
  Expect$Dart._fail$$member_('Expect.isNull(actual: <' + $toString(actual) + '>' + $toString(msg) + ') fails.');
}
;
Expect$Dart.isNull$named = function($n, $o, actual, reason){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 1:
      reason = $o.reason?(++seen , $o.reason):(++def , $Dart$Null);
  }
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Expect$Dart.isNull$member(actual, reason);
}
;
Expect$Dart.isNull$getter = function isNull$getter(){
  return Expect$Dart.isNull$named;
}
;
Expect$Dart.isNotNull$member = function(actual, reason){
  if (actual != null) {
    return;
  }
  var msg = Expect$Dart._getMessage$$member_(reason);
  Expect$Dart._fail$$member_('Expect.isNotNull(actual: <' + $toString(actual) + '>' + $toString(msg) + ') fails.');
}
;
Expect$Dart.isNotNull$named = function($n, $o, actual, reason){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 1:
      reason = $o.reason?(++seen , $o.reason):(++def , $Dart$Null);
  }
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Expect$Dart.isNotNull$member(actual, reason);
}
;
Expect$Dart.isNotNull$getter = function isNotNull$getter(){
  return Expect$Dart.isNotNull$named;
}
;
Expect$Dart.identical$member = function(expected, actual, reason){
  if (expected === actual) {
    return;
  }
  var msg = Expect$Dart._getMessage$$member_(reason);
  Expect$Dart._fail$$member_(ADD$operator('Expect.identical(expected: <' + $toString(expected) + '>, actual: <' + $toString(actual) + '>' + $toString(msg) + ') ', 'fails.'));
}
;
Expect$Dart.identical$named = function($n, $o, expected, actual, reason){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 2:
      reason = $o.reason?(++seen , $o.reason):(++def , $Dart$Null);
  }
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return Expect$Dart.identical$member(expected, actual, reason);
}
;
Expect$Dart.identical$getter = function identical$getter(){
  return Expect$Dart.identical$named;
}
;
Expect$Dart.fail$member = function(msg){
  Expect$Dart._fail$$member_("Expect.fail('" + $toString(msg) + "')");
}
;
Expect$Dart.fail$named = function($n, $o, msg){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Expect$Dart.fail$member(msg);
}
;
Expect$Dart.fail$getter = function fail$getter(){
  return Expect$Dart.fail$named;
}
;
Expect$Dart.approxEquals$member = function(expected, actual, tolerance, reason){
  if (EQ$operator(tolerance, $Dart$Null)) {
    tolerance = DIV$operator(expected, Math$Dart.pow$member(10, 4)).abs$named(0, $noargs);
  }
  if (LTE$operator(SUB$operator(expected, actual).abs$named(0, $noargs), tolerance)) {
    return;
  }
  var msg = Expect$Dart._getMessage$$member_(reason);
  Expect$Dart._fail$$member_(ADD$operator('Expect.approxEquals(expected:<' + $toString(expected) + '>, actual:<' + $toString(actual) + '>, ', 'tolerance:<' + $toString(tolerance) + '>' + $toString(msg) + ') fails'));
}
;
Expect$Dart.approxEquals$named = function($n, $o, expected, actual, tolerance, reason){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 2:
      tolerance = $o.tolerance?(++seen , $o.tolerance):(++def , $Dart$Null);
    case 3:
      reason = $o.reason?(++seen , $o.reason):(++def , $Dart$Null);
  }
  if (seen != $o.count || seen + def + $n != 4)
    $nsme();
  return Expect$Dart.approxEquals$member(expected, actual, tolerance, reason);
}
;
Expect$Dart.approxEquals$getter = function approxEquals$getter(){
  return Expect$Dart.approxEquals$named;
}
;
Expect$Dart.notEquals$member = function(unexpected, actual, reason){
  if (NE$operator(unexpected, actual)) {
    return;
  }
  var msg = Expect$Dart._getMessage$$member_(reason);
  Expect$Dart._fail$$member_(ADD$operator('Expect.notEquals(unexpected: <' + $toString(unexpected) + '>, actual:<' + $toString(actual) + '>' + $toString(msg) + ') ', 'fails.'));
}
;
Expect$Dart.notEquals$named = function($n, $o, unexpected, actual, reason){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 2:
      reason = $o.reason?(++seen , $o.reason):(++def , $Dart$Null);
  }
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return Expect$Dart.notEquals$member(unexpected, actual, reason);
}
;
Expect$Dart.notEquals$getter = function notEquals$getter(){
  return Expect$Dart.notEquals$named;
}
;
Expect$Dart.listEquals$member = function(expected, actual, reason){
  var tmp$0;
  var msg = Expect$Dart._getMessage$$member_(reason);
  var n = Math$Dart.min$member(expected.length$getter(), actual.length$getter());
  {
    var i = 0;
    for (; LT$operator(i, n); tmp$0 = i , (i = ADD$operator(tmp$0, 1) , tmp$0)) {
      if (NE$operator(expected.INDEX$operator(i), actual.INDEX$operator(i))) {
        Expect$Dart._fail$$member_(ADD$operator('Expect.listEquals(at index ' + $toString(i) + ', ', 'expected: <' + $toString(expected.INDEX$operator(0)) + '>, actual: <' + $toString(actual.INDEX$operator(i)) + '>' + $toString(msg) + ') fails'));
      }
    }
  }
  if (NE$operator(expected.length$getter(), actual.length$getter())) {
    Expect$Dart._fail$$member_(ADD$operator(ADD$operator('Expect.listEquals(list length, ', 'expected: <' + $toString(expected.length$getter()) + '>, actual: <' + $toString(actual.length$getter()) + '>' + $toString(msg) + ') '), 'fails'));
  }
}
;
Expect$Dart.listEquals$named = function($n, $o, expected, actual, reason){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 2:
      reason = $o.reason?(++seen , $o.reason):(++def , $Dart$Null);
  }
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return Expect$Dart.listEquals$member(expected, actual, reason);
}
;
Expect$Dart.listEquals$getter = function listEquals$getter(){
  return Expect$Dart.listEquals$named;
}
;
Expect$Dart.stringEquals$member = function(expected, actual, reason){
  var tmp$1, tmp$0;
  var msg = Expect$Dart._getMessage$$member_(reason);
  var defaultMessage = 'Expect.stringEquals(expected: <' + $toString(expected) + '>", <' + $toString(actual) + '>' + $toString(msg) + ') fails';
  if (NE$operator(expected, actual)) {
    var left = 0;
    var eLen = expected.length$getter();
    var aLen = actual.length$getter();
    while (true) {
      if (EQ$operator(left, eLen)) {
        assert(LT$operator(left, aLen));
        var snippet = actual.substring$named(2, $noargs, left, aLen);
        Expect$Dart._fail$$member_('' + $toString(defaultMessage) + '\nDiff:\n...[  ]\n...[ ' + $toString(snippet) + ' ]');
        return;
      }
      if (EQ$operator(left, aLen)) {
        assert(LT$operator(left, eLen));
        var snippet_0 = expected.substring$named(2, $noargs, left, eLen);
        Expect$Dart._fail$$member_('' + $toString(defaultMessage) + '\nDiff:\n...[  ]\n...[ ' + $toString(snippet_0) + ' ]');
        return;
      }
      if (NE$operator(expected.INDEX$operator(left), actual.INDEX$operator(left))) {
        break;
      }
      tmp$0 = left , (left = ADD$operator(tmp$0, 1) , tmp$0);
    }
    var right = 0;
    while (true) {
      if (EQ$operator(right, eLen)) {
        assert(LT$operator(right, aLen));
        var snippet_0_0 = actual.substring$named(2, $noargs, 0, SUB$operator(aLen, right));
        Expect$Dart._fail$$member_('' + $toString(defaultMessage) + '\nDiff:\n[  ]...\n[ ' + $toString(snippet_0_0) + ' ]...');
        return;
      }
      if (EQ$operator(right, aLen)) {
        assert(LT$operator(right, eLen));
        var snippet_1 = expected.substring$named(2, $noargs, 0, SUB$operator(eLen, right));
        Expect$Dart._fail$$member_('' + $toString(defaultMessage) + '\nDiff:\n[  ]...\n[ ' + $toString(snippet_1) + ' ]...');
        return;
      }
      if (NE$operator(expected.INDEX$operator(SUB$operator(SUB$operator(eLen, right), 1)), actual.INDEX$operator(SUB$operator(SUB$operator(aLen, right), 1)))) {
        break;
      }
      tmp$1 = right , (right = ADD$operator(tmp$1, 1) , tmp$1);
    }
    var eSnippet = expected.substring$named(2, $noargs, left, SUB$operator(eLen, right));
    var aSnippet = actual.substring$named(2, $noargs, left, SUB$operator(aLen, right));
    var diff = '\nDiff:\n...[ ' + $toString(eSnippet) + '} ]...\n...[ ' + $toString(aSnippet) + ' ]...';
    Expect$Dart._fail$$member_('' + $toString(defaultMessage) + '' + $toString(diff) + '');
  }
}
;
Expect$Dart.stringEquals$named = function($n, $o, expected, actual, reason){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 2:
      reason = $o.reason?(++seen , $o.reason):(++def , $Dart$Null);
  }
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return Expect$Dart.stringEquals$member(expected, actual, reason);
}
;
Expect$Dart.stringEquals$getter = function stringEquals$getter(){
  return Expect$Dart.stringEquals$named;
}
;
Expect$Dart.setEquals$member = function(expected, actual, reason){
  var missingSet = HashSetImplementation$Dart.HashSetImplementation$from$21$Factory(expected);
  missingSet.removeAll$named(1, $noargs, actual);
  var extraSet = HashSetImplementation$Dart.HashSetImplementation$from$21$Factory(actual);
  extraSet.removeAll$named(1, $noargs, expected);
  if (extraSet.isEmpty$named(0, $noargs) && missingSet.isEmpty$named(0, $noargs)) {
    return;
  }
  var msg = Expect$Dart._getMessage$$member_(reason);
  var sb = StringBufferImpl$Dart.StringBufferImpl$$Factory('Expect.setEquals(' + $toString(msg) + ') fails');
  if (!missingSet.isEmpty$named(0, $noargs)) {
    sb.add$named(1, $noargs, '\nExpected collection does not contain: ');
  }
  {
    var $0 = missingSet.iterator$named(0, $noargs);
    while ($0.hasNext$named(0, $noargs)) {
      var val = $0.next$named(0, $noargs);
      {
        sb.add$named(1, $noargs, '' + $toString(val) + ' ');
      }
    }
  }
  if (!extraSet.isEmpty$named(0, $noargs)) {
    sb.add$named(1, $noargs, '\nExpected collection should not contain: ');
  }
  {
    var $1 = extraSet.iterator$named(0, $noargs);
    while ($1.hasNext$named(0, $noargs)) {
      var val_1 = $1.next$named(0, $noargs);
      {
        sb.add$named(1, $noargs, '' + $toString(val_1) + ' ');
      }
    }
  }
  Expect$Dart._fail$$member_(sb.toString$named(0, $noargs));
}
;
Expect$Dart.setEquals$named = function($n, $o, expected, actual, reason){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 2:
      reason = $o.reason?(++seen , $o.reason):(++def , $Dart$Null);
  }
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return Expect$Dart.setEquals$member(expected, actual, reason);
}
;
Expect$Dart.setEquals$getter = function setEquals$getter(){
  return Expect$Dart.setEquals$named;
}
;
Expect$Dart.throws$member = function(f, check, reason){
  var e_0;
  try {
    f(0, $noargs);
  }
   catch (e) {
    e = $transformBrowserException(e);
    {
      e_0 = e;
      if (NE$operator(check, $Dart$Null)) {
        Expect$Dart.isTrue$member(check(1, $noargs, e_0), $Dart$Null);
      }
      return;
    }
  }
  var msg = Expect$Dart._getMessage$$member_(reason);
  Expect$Dart._fail$$member_('Expect.throws(' + $toString(msg) + ') fails');
}
;
Expect$Dart.throws$named = function($n, $o, f, check, reason){
  var seen = 0;
  var def = 0;
  switch ($n) {
    case 1:
      check = $o.check?(++seen , $o.check):(++def , $Dart$Null);
    case 2:
      reason = $o.reason?(++seen , $o.reason):(++def , $Dart$Null);
  }
  if (seen != $o.count || seen + def + $n != 3)
    $nsme();
  return Expect$Dart.throws$member(f, check, reason);
}
;
Expect$Dart.throws$getter = function throws$getter(){
  return Expect$Dart.throws$named;
}
;
Expect$Dart._getMessage$$member_ = function(reason){
  return EQ$operator(reason, $Dart$Null)?'':", '" + $toString(reason) + "'";
}
;
Expect$Dart._getMessage$$named_ = function($n, $o, reason){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Expect$Dart._getMessage$$member_(reason);
}
;
Expect$Dart._getMessage$$getter_ = function _getMessage$$getter_(){
  return Expect$Dart._getMessage$$named_;
}
;
Expect$Dart._fail$$member_ = function(message){
  $Dart$ThrowException(ExpectException$Dart.ExpectException$$Factory(message));
}
;
Expect$Dart._fail$$named_ = function($n, $o, message){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Expect$Dart._fail$$member_(message);
}
;
Expect$Dart._fail$$getter_ = function _fail$$getter_(){
  return Expect$Dart._fail$$named_;
}
;
Expect$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
Expect$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
Expect$Dart.Expect$$Factory = function(){
  var tmp$0 = new Expect$Dart;
  tmp$0.$typeInfo = Expect$Dart.$lookupRTT();
  Expect$Dart.$Initializer.call(tmp$0);
  Expect$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function ExpectException$Dart(){
}

ExpectException$Dart.$lookupRTT = function(){
  return RTT.create($cls('ExpectException$Dart'), ExpectException$Dart.$RTTimplements);
}
;
ExpectException$Dart.$RTTimplements = function(rtt){
  ExpectException$Dart.$addTo(rtt);
}
;
ExpectException$Dart.$addTo = function(target){
  var rtt = ExpectException$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Exception$Dart.$addTo(target);
}
;
ExpectException$Dart.prototype.$implements$ExpectException$Dart = 1;
ExpectException$Dart.prototype.$implements$Exception$Dart = 1;
ExpectException$Dart.prototype.$implements$Object$Dart = 1;
ExpectException$Dart.$Constructor = function(message){
  Object.$Constructor.call(this);
}
;
ExpectException$Dart.$Initializer = function(message){
  Object.$Initializer.call(this);
  this.message$field = message;
}
;
ExpectException$Dart.ExpectException$$Factory = function(message){
  var tmp$0 = new ExpectException$Dart;
  tmp$0.$typeInfo = ExpectException$Dart.$lookupRTT();
  ExpectException$Dart.$Initializer.call(tmp$0, message);
  ExpectException$Dart.$Constructor.call(tmp$0, message);
  return tmp$0;
}
;
ExpectException$Dart.prototype.toString$member = function(){
  return this.message$getter();
}
;
ExpectException$Dart.prototype.toString$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return ExpectException$Dart.prototype.toString$member.call(this);
}
;
ExpectException$Dart.prototype.toString$getter = function toString$getter(){
  return $bind(ExpectException$Dart.prototype.toString$named, this);
}
;
ExpectException$Dart.prototype.message$named = function(){
  return this.message$getter().apply(this, arguments);
}
;
ExpectException$Dart.prototype.message$getter = function(){
  return this.message$field;
}
;
ExpectException$Dart.prototype.message$setter = function(tmp$0){
  this.message$field = tmp$0;
}
;
function Function$Dart(){
}

Function$Dart.$lookupRTT = function(){
  return RTT.create($cls('Function$Dart'));
}
;
Function$Dart.$addTo = function(target){
  var rtt = Function$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
function Hashable$Dart(){
}

Hashable$Dart.$lookupRTT = function(){
  return RTT.create($cls('Hashable$Dart'));
}
;
Hashable$Dart.$addTo = function(target){
  var rtt = Hashable$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
function int$Dart(){
}

int$Dart.$lookupRTT = function(){
  return RTT.create($cls('int$Dart'), int$Dart.$RTTimplements);
}
;
int$Dart.$RTTimplements = function(rtt){
  int$Dart.$addTo(rtt);
}
;
int$Dart.$addTo = function(target){
  var rtt = int$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  num$Dart.$addTo(target);
}
;
function SendPort$Dart(){
}

SendPort$Dart.$lookupRTT = function(){
  return RTT.create($cls('SendPort$Dart'), SendPort$Dart.$RTTimplements);
}
;
SendPort$Dart.$RTTimplements = function(rtt){
  SendPort$Dart.$addTo(rtt);
}
;
SendPort$Dart.$addTo = function(target){
  var rtt = SendPort$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Hashable$Dart.$addTo(target);
}
;
function ReceivePort$Dart(){
}

ReceivePort$Dart.$lookupRTT = function(){
  return RTT.create($cls('ReceivePort$Dart'));
}
;
ReceivePort$Dart.$addTo = function(target){
  var rtt = ReceivePort$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
function Isolate$Dart(){
}

Isolate$Dart.$lookupRTT = function(){
  return RTT.create($cls('Isolate$Dart'));
}
;
Isolate$Dart.$addTo = function(target){
  var rtt = Isolate$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
Isolate$Dart.prototype.$implements$Isolate$Dart = 1;
Isolate$Dart.prototype.$implements$Object$Dart = 1;
function Isolate$Dart$isolateFactory(){
  return Isolate$Dart.default$factory();
}

Isolate$Dart.prototype.getIsolateFactory = function(){
  return Isolate$Dart$isolateFactory;
}
;
Isolate$Dart.$Constructor = function(){
  Isolate$Dart.light$Constructor.call(this);
}
;
Isolate$Dart.$Initializer = function(){
  Isolate$Dart.light$Initializer.call(this);
}
;
Isolate$Dart.Isolate$$Factory = function(){
  var tmp$0 = new Isolate$Dart;
  tmp$0.$typeInfo = Isolate$Dart.$lookupRTT();
  Isolate$Dart.$Initializer.call(tmp$0);
  Isolate$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
Isolate$Dart.default$factory = Isolate$Dart.Isolate$$Factory;
Isolate$Dart.light$Constructor = function(){
  Object.$Constructor.call(this);
}
;
Isolate$Dart.light$Initializer = function(){
  Object.$Initializer.call(this);
  this._isLight$$field_ = true;
}
;
Isolate$Dart.Isolate$light$7$Factory = function(){
  var tmp$0 = new Isolate$Dart;
  tmp$0.$typeInfo = Isolate$Dart.$lookupRTT();
  Isolate$Dart.light$Initializer.call(tmp$0);
  Isolate$Dart.light$Constructor.call(tmp$0);
  return tmp$0;
}
;
Isolate$Dart.heavy$Constructor = function(){
  Object.$Constructor.call(this);
}
;
Isolate$Dart.heavy$Initializer = function(){
  Object.$Initializer.call(this);
  this._isLight$$field_ = false;
}
;
Isolate$Dart.Isolate$heavy$7$Factory = function(){
  var tmp$0 = new Isolate$Dart;
  tmp$0.$typeInfo = Isolate$Dart.$lookupRTT();
  Isolate$Dart.heavy$Initializer.call(tmp$0);
  Isolate$Dart.heavy$Constructor.call(tmp$0);
  return tmp$0;
}
;
Isolate$Dart.prototype.spawn$member = function(){
  return IsolateNatives$Dart.spawn$member(this, this._isLight$$getter_());
}
;
Isolate$Dart.prototype.spawn$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Isolate$Dart.prototype.spawn$member.call(this);
}
;
Isolate$Dart.prototype.spawn$getter = function spawn$getter(){
  return $bind(Isolate$Dart.prototype.spawn$named, this);
}
;
Isolate$Dart.prototype._run$$member_ = function(port){
  var tmp$0;
  this._port$$setter_(tmp$0 = port) , tmp$0;
  this.main$member();
}
;
Isolate$Dart.prototype._run$$named_ = function($n, $o, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Isolate$Dart.prototype._run$$member_.call(this, port);
}
;
Isolate$Dart.prototype._run$$getter_ = function _run$$getter_(){
  return $bind(Isolate$Dart.prototype._run$$named_, this);
}
;
Isolate$Dart.prototype.port$named = function(){
  return this.port$getter().apply(this, arguments);
}
;
Isolate$Dart.prototype.port$getter = function(){
  return this._port$$getter_();
}
;
Isolate$Dart.prototype.main$member = function(){
}
;
Isolate$Dart.prototype.main$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Isolate$Dart.prototype.main$member.call(this);
}
;
Isolate$Dart.prototype.main$getter = function main$getter(){
  return $bind(Isolate$Dart.prototype.main$named, this);
}
;
Isolate$Dart.prototype._isLight$$named_ = function(){
  return this._isLight$$getter_().apply(this, arguments);
}
;
Isolate$Dart.prototype._isLight$$getter_ = function(){
  return this._isLight$$field_;
}
;
Isolate$Dart.prototype._port$$named_ = function(){
  return this._port$$getter_().apply(this, arguments);
}
;
Isolate$Dart.prototype._port$$getter_ = function(){
  return this._port$$field_;
}
;
Isolate$Dart.prototype._port$$setter_ = function(tmp$0){
  this._port$$field_ = tmp$0;
}
;
Isolate$Dart.bind$member = function(f){
  return IsolateNatives$Dart.bind$member(f);
}
;
Isolate$Dart.bind$named = function($n, $o, f){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Isolate$Dart.bind$member(f);
}
;
Isolate$Dart.bind$getter = function bind$getter(){
  return Isolate$Dart.bind$named;
}
;
function Iterable$Dart(){
}

Iterable$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('Iterable$Dart'), null, typeArgs);
}
;
Iterable$Dart.$addTo = function(target, typeArgs){
  var rtt = Iterable$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
}
;
function Iterator$Dart(){
}

Iterator$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('Iterator$Dart'), null, typeArgs);
}
;
Iterator$Dart.$addTo = function(target, typeArgs){
  var rtt = Iterator$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
}
;
function List$Dart(){
}

List$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('List$Dart'), List$Dart.$RTTimplements, typeArgs);
}
;
List$Dart.$RTTimplements = function(rtt, typeArgs){
  List$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
List$Dart.$addTo = function(target, typeArgs){
  var rtt = List$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  Collection$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0)]);
}
;
function Map$Dart(){
}

Map$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('Map$Dart'), null, typeArgs);
}
;
Map$Dart.$addTo = function(target, typeArgs){
  var rtt = Map$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
}
;
function HashMap$Dart(){
}

HashMap$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('HashMap$Dart'), HashMap$Dart.$RTTimplements, typeArgs);
}
;
HashMap$Dart.$RTTimplements = function(rtt, typeArgs){
  HashMap$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
HashMap$Dart.$addTo = function(target, typeArgs){
  var rtt = HashMap$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  Map$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0), RTT.getTypeArg(target.typeArgs, 1)]);
}
;
function LinkedHashMap$Dart(){
}

LinkedHashMap$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('LinkedHashMap$Dart'), LinkedHashMap$Dart.$RTTimplements, typeArgs);
}
;
LinkedHashMap$Dart.$RTTimplements = function(rtt, typeArgs){
  LinkedHashMap$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
LinkedHashMap$Dart.$addTo = function(target, typeArgs){
  var rtt = LinkedHashMap$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  HashMap$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0), RTT.getTypeArg(target.typeArgs, 1)]);
}
;
function Math$Dart(){
}

Math$Dart.$lookupRTT = function(){
  return RTT.create($cls('Math$Dart'));
}
;
Math$Dart.$addTo = function(target){
  var rtt = Math$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
Math$Dart.prototype.$implements$Math$Dart = 1;
Math$Dart.prototype.$implements$Object$Dart = 1;
Math$Dart.E$named = function(){
  return Math$Dart.E$getter().apply(this, arguments);
}
;
Math$Dart.E$getter = function(){
  return 2.718281828459045;
}
;
Math$Dart.LN10$named = function(){
  return Math$Dart.LN10$getter().apply(this, arguments);
}
;
Math$Dart.LN10$getter = function(){
  return 2.302585092994046;
}
;
Math$Dart.LN2$named = function(){
  return Math$Dart.LN2$getter().apply(this, arguments);
}
;
Math$Dart.LN2$getter = function(){
  return 0.6931471805599453;
}
;
Math$Dart.LOG2E$named = function(){
  return Math$Dart.LOG2E$getter().apply(this, arguments);
}
;
Math$Dart.LOG2E$getter = function(){
  return 1.4426950408889634;
}
;
Math$Dart.LOG10E$named = function(){
  return Math$Dart.LOG10E$getter().apply(this, arguments);
}
;
Math$Dart.LOG10E$getter = function(){
  return 0.4342944819032518;
}
;
Math$Dart.PI$named = function(){
  return Math$Dart.PI$getter().apply(this, arguments);
}
;
Math$Dart.PI$getter = function(){
  return 3.141592653589793;
}
;
Math$Dart.SQRT1_2$named = function(){
  return Math$Dart.SQRT1_2$getter().apply(this, arguments);
}
;
Math$Dart.SQRT1_2$getter = function(){
  return 0.7071067811865476;
}
;
Math$Dart.SQRT2$named = function(){
  return Math$Dart.SQRT2$getter().apply(this, arguments);
}
;
Math$Dart.SQRT2$getter = function(){
  return 1.4142135623730951;
}
;
Math$Dart.parseInt$member = function(str){
  return MathNatives$Dart.parseInt$member(str);
}
;
Math$Dart.parseInt$named = function($n, $o, str){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Math$Dart.parseInt$member(str);
}
;
Math$Dart.parseInt$getter = function parseInt$getter(){
  return Math$Dart.parseInt$named;
}
;
Math$Dart.parseDouble$member = function(str){
  return MathNatives$Dart.parseDouble$member(str);
}
;
Math$Dart.parseDouble$named = function($n, $o, str){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Math$Dart.parseDouble$member(str);
}
;
Math$Dart.parseDouble$getter = function parseDouble$getter(){
  return Math$Dart.parseDouble$named;
}
;
Math$Dart.min$member = function(a, b){
  var c = a.compareTo$named(1, $noargs, b);
  if (EQ$operator(c, 0)) {
    return a;
  }
  if (LT$operator(c, 0)) {
    if (Number.$instanceOf(b) && b.isNaN$named(0, $noargs)) {
      return b;
    }
    return a;
  }
  if (Number.$instanceOf(a) && a.isNaN$named(0, $noargs)) {
    return a;
  }
  return b;
}
;
Math$Dart.min$named = function($n, $o, a, b){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Math$Dart.min$member(a, b);
}
;
Math$Dart.min$getter = function min$getter(){
  return Math$Dart.min$named;
}
;
Math$Dart.max$member = function(a, b){
  return LT$operator(a.compareTo$named(1, $noargs, b), 0)?b:a;
}
;
Math$Dart.max$named = function($n, $o, a, b){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Math$Dart.max$member(a, b);
}
;
Math$Dart.max$getter = function max$getter(){
  return Math$Dart.max$named;
}
;
Math$Dart.atan2$member = function(a, b){
  return MathNatives$Dart.atan2$member(a, b);
}
;
Math$Dart.atan2$named = function($n, $o, a, b){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Math$Dart.atan2$member(a, b);
}
;
Math$Dart.atan2$getter = function atan2$getter(){
  return Math$Dart.atan2$named;
}
;
Math$Dart.pow$member = function(x, exponent){
  return MathNatives$Dart.pow$member(x, exponent);
}
;
Math$Dart.pow$named = function($n, $o, x, exponent){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Math$Dart.pow$member(x, exponent);
}
;
Math$Dart.pow$getter = function pow$getter(){
  return Math$Dart.pow$named;
}
;
Math$Dart.random$member = function(){
  return MathNatives$Dart.random$member();
}
;
Math$Dart.random$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return Math$Dart.random$member();
}
;
Math$Dart.random$getter = function random$getter(){
  return Math$Dart.random$named;
}
;
Math$Dart.sin$member = function(x){
  return MathNatives$Dart.sin$member(x);
}
;
Math$Dart.sin$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Math$Dart.sin$member(x);
}
;
Math$Dart.sin$getter = function sin$getter(){
  return Math$Dart.sin$named;
}
;
Math$Dart.cos$member = function(x){
  return MathNatives$Dart.cos$member(x);
}
;
Math$Dart.cos$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Math$Dart.cos$member(x);
}
;
Math$Dart.cos$getter = function cos$getter(){
  return Math$Dart.cos$named;
}
;
Math$Dart.tan$member = function(x){
  return MathNatives$Dart.tan$member(x);
}
;
Math$Dart.tan$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Math$Dart.tan$member(x);
}
;
Math$Dart.tan$getter = function tan$getter(){
  return Math$Dart.tan$named;
}
;
Math$Dart.acos$member = function(x){
  return MathNatives$Dart.acos$member(x);
}
;
Math$Dart.acos$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Math$Dart.acos$member(x);
}
;
Math$Dart.acos$getter = function acos$getter(){
  return Math$Dart.acos$named;
}
;
Math$Dart.asin$member = function(x){
  return MathNatives$Dart.asin$member(x);
}
;
Math$Dart.asin$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Math$Dart.asin$member(x);
}
;
Math$Dart.asin$getter = function asin$getter(){
  return Math$Dart.asin$named;
}
;
Math$Dart.atan$member = function(x){
  return MathNatives$Dart.atan$member(x);
}
;
Math$Dart.atan$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Math$Dart.atan$member(x);
}
;
Math$Dart.atan$getter = function atan$getter(){
  return Math$Dart.atan$named;
}
;
Math$Dart.sqrt$member = function(x){
  return MathNatives$Dart.sqrt$member(x);
}
;
Math$Dart.sqrt$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Math$Dart.sqrt$member(x);
}
;
Math$Dart.sqrt$getter = function sqrt$getter(){
  return Math$Dart.sqrt$named;
}
;
Math$Dart.exp$member = function(x){
  return MathNatives$Dart.exp$member(x);
}
;
Math$Dart.exp$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Math$Dart.exp$member(x);
}
;
Math$Dart.exp$getter = function exp$getter(){
  return Math$Dart.exp$named;
}
;
Math$Dart.log$member = function(x){
  return MathNatives$Dart.log$member(x);
}
;
Math$Dart.log$named = function($n, $o, x){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Math$Dart.log$member(x);
}
;
Math$Dart.log$getter = function log$getter(){
  return Math$Dart.log$named;
}
;
Math$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
Math$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
Math$Dart.Math$$Factory = function(){
  var tmp$0 = new Math$Dart;
  tmp$0.$typeInfo = Math$Dart.$lookupRTT();
  Math$Dart.$Initializer.call(tmp$0);
  Math$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function num$Dart(){
}

num$Dart.$lookupRTT = function(){
  return RTT.create($cls('num$Dart'), num$Dart.$RTTimplements);
}
;
num$Dart.$RTTimplements = function(rtt){
  num$Dart.$addTo(rtt);
}
;
num$Dart.$addTo = function(target){
  var rtt = num$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Comparable$Dart.$addTo(target);
  Hashable$Dart.$addTo(target);
}
;
function Pattern$Dart(){
}

Pattern$Dart.$lookupRTT = function(){
  return RTT.create($cls('Pattern$Dart'));
}
;
Pattern$Dart.$addTo = function(target){
  var rtt = Pattern$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
function Promise$Dart(){
}

Promise$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('Promise$Dart'), null, typeArgs);
}
;
Promise$Dart.$addTo = function(target, typeArgs){
  var rtt = Promise$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
}
;
function Proxy$Dart(){
}

Proxy$Dart.$lookupRTT = function(){
  return RTT.create($cls('Proxy$Dart'), Proxy$Dart.$RTTimplements);
}
;
Proxy$Dart.$RTTimplements = function(rtt){
  Proxy$Dart.$addTo(rtt);
}
;
Proxy$Dart.$addTo = function(target){
  var rtt = Proxy$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  ProxyImpl$Dart.$addTo(target);
}
;
Proxy$Dart.prototype.$implements$Proxy$Dart = 1;
Proxy$Dart.prototype.$implements$ProxyImpl$Dart = 1;
Proxy$Dart.prototype.$implements$Object$Dart = 1;
$inherits(Proxy$Dart, ProxyImpl$Dart);
Proxy$Dart.forPort$Constructor = function(port){
  ProxyImpl$Dart.forPort$Constructor.call(this, port);
}
;
Proxy$Dart.forPort$Initializer = function(port){
  ProxyImpl$Dart.forPort$Initializer.call(this, port);
}
;
Proxy$Dart.Proxy$forPort$5$Factory = function(port){
  var tmp$0 = new Proxy$Dart;
  tmp$0.$typeInfo = Proxy$Dart.$lookupRTT();
  Proxy$Dart.forPort$Initializer.call(tmp$0, port);
  Proxy$Dart.forPort$Constructor.call(tmp$0, port);
  return tmp$0;
}
;
Proxy$Dart.forIsolate$Constructor = function(isolate){
  Proxy$Dart._forIsolateWithPromise$$Constructor_.call(this, isolate, PromiseImpl$Dart.PromiseImpl$$Factory(PromiseImpl$Dart.$lookupRTT([SendPort$Dart.$lookupRTT()])));
}
;
Proxy$Dart.forIsolate$Initializer = function(isolate){
  Proxy$Dart._forIsolateWithPromise$$Initializer_.call(this, isolate, PromiseImpl$Dart.PromiseImpl$$Factory(PromiseImpl$Dart.$lookupRTT([SendPort$Dart.$lookupRTT()])));
}
;
Proxy$Dart.Proxy$forIsolate$5$Factory = function(isolate){
  var tmp$0 = new Proxy$Dart;
  tmp$0.$typeInfo = Proxy$Dart.$lookupRTT();
  Proxy$Dart.forIsolate$Initializer.call(tmp$0, isolate);
  Proxy$Dart.forIsolate$Constructor.call(tmp$0, isolate);
  return tmp$0;
}
;
function Proxy$Dart$_forIsolateWithPromise$c0$10_10$HoistedConstructor(dartc_scp$0, port){
  dartc_scp$0.promise.complete$named(1, $noargs, port);
}

function Proxy$Dart$_forIsolateWithPromise$c0$10_10$HoistedConstructor$named($s0, $n, $o, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Proxy$Dart$_forIsolateWithPromise$c0$10_10$HoistedConstructor($s0, port);
}

Proxy$Dart._forIsolateWithPromise$$Constructor_ = function(isolate, promise){
  var dartc_scp$0 = {promise:promise};
  ProxyImpl$Dart.forReply$Constructor.call(this, dartc_scp$0.promise);
  isolate.spawn$named(0, $noargs).then$named(1, $noargs, $bind(Proxy$Dart$_forIsolateWithPromise$c0$10_10$HoistedConstructor$named, $Dart$Null, dartc_scp$0));
}
;
Proxy$Dart._forIsolateWithPromise$$Initializer_ = function(isolate, promise){
  var dartc_scp$0 = {promise:promise};
  ProxyImpl$Dart.forReply$Initializer.call(this, dartc_scp$0.promise);
}
;
Proxy$Dart.Proxy$_forIsolateWithPromise$5$$Factory_ = function(isolate, promise){
  var tmp$0 = new Proxy$Dart;
  tmp$0.$typeInfo = Proxy$Dart.$lookupRTT();
  Proxy$Dart._forIsolateWithPromise$$Initializer_.call(tmp$0, isolate, promise);
  Proxy$Dart._forIsolateWithPromise$$Constructor_.call(tmp$0, isolate, promise);
  return tmp$0;
}
;
Proxy$Dart.forReply$Constructor = function(port){
  ProxyImpl$Dart.forReply$Constructor.call(this, port);
}
;
Proxy$Dart.forReply$Initializer = function(port){
  ProxyImpl$Dart.forReply$Initializer.call(this, port);
}
;
Proxy$Dart.Proxy$forReply$5$Factory = function(port){
  var tmp$0 = new Proxy$Dart;
  tmp$0.$typeInfo = Proxy$Dart.$lookupRTT();
  Proxy$Dart.forReply$Initializer.call(tmp$0, port);
  Proxy$Dart.forReply$Constructor.call(tmp$0, port);
  return tmp$0;
}
;
function Dispatcher$Dart(){
}

Dispatcher$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('Dispatcher$Dart'), null, typeArgs);
}
;
Dispatcher$Dart.$addTo = function(target, typeArgs){
  var rtt = Dispatcher$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
}
;
Dispatcher$Dart.prototype.$implements$Dispatcher$Dart = 1;
Dispatcher$Dart.prototype.$implements$Object$Dart = 1;
Dispatcher$Dart.$Constructor = function(target){
  Object.$Constructor.call(this);
}
;
Dispatcher$Dart.$Initializer = function(target){
  Object.$Initializer.call(this);
  this.target$field = target;
}
;
Dispatcher$Dart.Dispatcher$$Factory = function($rtt, target){
  var tmp$0 = new Dispatcher$Dart;
  tmp$0.$typeInfo = $rtt;
  Dispatcher$Dart.$Initializer.call(tmp$0, target);
  Dispatcher$Dart.$Constructor.call(tmp$0, target);
  return tmp$0;
}
;
function Dispatcher$Dart$_serve$c0$reply$15_6_2$Hoisted(dartc_scp$3, response){
  var proxy = Proxy$Dart.Proxy$forPort$5$Factory(dartc_scp$3.replyTo);
  proxy.send$named(1, $noargs, RTT.setTypeInfo([response], Array.$lookupRTT()));
}

function Dispatcher$Dart$_serve$c0$reply$15_6_2$Hoisted$named($s0, $n, $o, response){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Dispatcher$Dart$_serve$c0$reply$15_6_2$Hoisted($s0, response);
}

function Dispatcher$Dart$_serve$c1$15_15$Hoisted(message, replyTo){
  var dartc_scp$3 = {replyTo:replyTo};
  this.process$named(2, $noargs, message, $bind(Dispatcher$Dart$_serve$c0$reply$15_6_2$Hoisted$named, $Dart$Null, dartc_scp$3));
}

function Dispatcher$Dart$_serve$c1$15_15$Hoisted$named($n, $o, message, replyTo){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Dispatcher$Dart$_serve$c1$15_15$Hoisted.call(this, message, replyTo);
}

Dispatcher$Dart.prototype._serve$$member_ = function(port){
  port.receive$named(1, $noargs, $bind(Dispatcher$Dart$_serve$c1$15_15$Hoisted$named, this));
}
;
Dispatcher$Dart.prototype._serve$$named_ = function($n, $o, port){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Dispatcher$Dart.prototype._serve$$member_.call(this, port);
}
;
Dispatcher$Dart.prototype._serve$$getter_ = function _serve$$getter_(){
  return $bind(Dispatcher$Dart.prototype._serve$$named_, this);
}
;
Dispatcher$Dart.serve$member = function(dispatcher){
  var port = ProxyImpl$Dart.register$member(dispatcher);
  dispatcher._serve$$named_(1, $noargs, port);
  return port.toSendPort$named(0, $noargs);
}
;
Dispatcher$Dart.serve$named = function($n, $o, dispatcher){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Dispatcher$Dart.serve$member(dispatcher);
}
;
Dispatcher$Dart.serve$getter = function serve$getter(){
  return Dispatcher$Dart.serve$named;
}
;
Dispatcher$Dart.prototype.process$member = function(message, reply){
  $Dart$ThrowException('Abstract method called');
}
;
Dispatcher$Dart.prototype.process$named = function($n, $o, message, reply){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Dispatcher$Dart.prototype.process$member.call(this, message, reply);
}
;
Dispatcher$Dart.prototype.process$getter = function process$getter(){
  return $bind(Dispatcher$Dart.prototype.process$named, this);
}
;
Dispatcher$Dart.prototype.target$named = function(){
  return this.target$getter().apply(this, arguments);
}
;
Dispatcher$Dart.prototype.target$getter = function(){
  return this.target$field;
}
;
Dispatcher$Dart.prototype.target$setter = function(tmp$0){
  this.target$field = tmp$0;
}
;
function Queue$Dart(){
}

Queue$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('Queue$Dart'), Queue$Dart.$RTTimplements, typeArgs);
}
;
Queue$Dart.$RTTimplements = function(rtt, typeArgs){
  Queue$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
Queue$Dart.$addTo = function(target, typeArgs){
  var rtt = Queue$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  Collection$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0)]);
}
;
function Match$Dart(){
}

Match$Dart.$lookupRTT = function(){
  return RTT.create($cls('Match$Dart'));
}
;
Match$Dart.$addTo = function(target){
  var rtt = Match$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
function RegExp$Dart(){
}

RegExp$Dart.$lookupRTT = function(){
  return RTT.create($cls('RegExp$Dart'), RegExp$Dart.$RTTimplements);
}
;
RegExp$Dart.$RTTimplements = function(rtt){
  RegExp$Dart.$addTo(rtt);
}
;
RegExp$Dart.$addTo = function(target){
  var rtt = RegExp$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Pattern$Dart.$addTo(target);
}
;
function Set$Dart(){
}

Set$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('Set$Dart'), Set$Dart.$RTTimplements, typeArgs);
}
;
Set$Dart.$RTTimplements = function(rtt, typeArgs){
  Set$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
Set$Dart.$addTo = function(target, typeArgs){
  var rtt = Set$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  Collection$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0)]);
}
;
function HashSet$Dart(){
}

HashSet$Dart.$lookupRTT = function(typeArgs){
  return RTT.create($cls('HashSet$Dart'), HashSet$Dart.$RTTimplements, typeArgs);
}
;
HashSet$Dart.$RTTimplements = function(rtt, typeArgs){
  HashSet$Dart.$addTo(rtt, typeArgs);
  rtt.derivedTypes = [];
}
;
HashSet$Dart.$addTo = function(target, typeArgs){
  var rtt = HashSet$Dart.$lookupRTT(typeArgs);
  target.implementedTypes[rtt.classKey] = rtt;
  Set$Dart.$addTo(target, [RTT.getTypeArg(target.typeArgs, 0)]);
}
;
function StopWatch$Dart(){
}

StopWatch$Dart.$lookupRTT = function(){
  return RTT.create($cls('StopWatch$Dart'));
}
;
StopWatch$Dart.$addTo = function(target){
  var rtt = StopWatch$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
function String$Dart(){
}

String$Dart.$lookupRTT = function(){
  return RTT.create($cls('String$Dart'), String$Dart.$RTTimplements);
}
;
String$Dart.$RTTimplements = function(rtt){
  String$Dart.$addTo(rtt);
}
;
String$Dart.$addTo = function(target){
  var rtt = String$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
  Comparable$Dart.$addTo(target);
  Hashable$Dart.$addTo(target);
  Pattern$Dart.$addTo(target);
}
;
function StringBuffer$Dart(){
}

StringBuffer$Dart.$lookupRTT = function(){
  return RTT.create($cls('StringBuffer$Dart'));
}
;
StringBuffer$Dart.$addTo = function(target){
  var rtt = StringBuffer$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
function Strings$Dart(){
}

Strings$Dart.$lookupRTT = function(){
  return RTT.create($cls('Strings$Dart'));
}
;
Strings$Dart.$addTo = function(target){
  var rtt = Strings$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
Strings$Dart.prototype.$implements$Strings$Dart = 1;
Strings$Dart.prototype.$implements$Object$Dart = 1;
Strings$Dart.String$fromCharCodes$6$Factory = function(charCodes){
  return StringBase$Dart.createFromCharCodes$member(charCodes);
}
;
Strings$Dart.join$member = function(strings, separator){
  return StringBase$Dart.join$member(strings, separator);
}
;
Strings$Dart.join$named = function($n, $o, strings, separator){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 2)
    $nsme();
  return Strings$Dart.join$member(strings, separator);
}
;
Strings$Dart.join$getter = function join$getter(){
  return Strings$Dart.join$named;
}
;
Strings$Dart.concatAll$member = function(strings){
  return StringBase$Dart.concatAll$member(strings);
}
;
Strings$Dart.concatAll$named = function($n, $o, strings){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 1)
    $nsme();
  return Strings$Dart.concatAll$member(strings);
}
;
Strings$Dart.concatAll$getter = function concatAll$getter(){
  return Strings$Dart.concatAll$named;
}
;
function TimeZone$Dart(){
}

TimeZone$Dart.$lookupRTT = function(){
  return RTT.create($cls('TimeZone$Dart'));
}
;
TimeZone$Dart.$addTo = function(target){
  var rtt = TimeZone$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
function unnamedcac5e9$HelloDartTest$Dart(){
}

unnamedcac5e9$HelloDartTest$Dart.$lookupRTT = function(){
  return RTT.create($cls('unnamedcac5e9$HelloDartTest$Dart'));
}
;
unnamedcac5e9$HelloDartTest$Dart.$addTo = function(target){
  var rtt = unnamedcac5e9$HelloDartTest$Dart.$lookupRTT();
  target.implementedTypes[rtt.classKey] = rtt;
}
;
unnamedcac5e9$HelloDartTest$Dart.prototype.$implements$unnamedcac5e9$HelloDartTest$Dart = 1;
unnamedcac5e9$HelloDartTest$Dart.prototype.$implements$Object$Dart = 1;
unnamedcac5e9$HelloDartTest$Dart.testMain$member = function(){
  print$getter()(1, $noargs, 'Hello, Darter!');
}
;
unnamedcac5e9$HelloDartTest$Dart.testMain$named = function($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return unnamedcac5e9$HelloDartTest$Dart.testMain$member();
}
;
unnamedcac5e9$HelloDartTest$Dart.testMain$getter = function testMain$getter(){
  return unnamedcac5e9$HelloDartTest$Dart.testMain$named;
}
;
unnamedcac5e9$HelloDartTest$Dart.$Constructor = function(){
  Object.$Constructor.call(this);
}
;
unnamedcac5e9$HelloDartTest$Dart.$Initializer = function(){
  Object.$Initializer.call(this);
}
;
unnamedcac5e9$HelloDartTest$Dart.HelloDartTest$$Factory = function(){
  var tmp$0 = new unnamedcac5e9$HelloDartTest$Dart;
  tmp$0.$typeInfo = unnamedcac5e9$HelloDartTest$Dart.$lookupRTT();
  unnamedcac5e9$HelloDartTest$Dart.$Initializer.call(tmp$0);
  unnamedcac5e9$HelloDartTest$Dart.$Constructor.call(tmp$0);
  return tmp$0;
}
;
function unnamedcac5e9$main$member(){
  unnamedcac5e9$HelloDartTest$Dart.testMain$member();
}

function unnamedcac5e9$main$named($n, $o){
  var seen = 0;
  var def = 0;
  if (seen != $o.count || seen + def + $n != 0)
    $nsme();
  return unnamedcac5e9$main$member();
}

isolate$inits.push(function(){
  this._callback$$field_ = static$uninitialized;
  isolate$current.ReceivePortImpl$Dart_nextFreeId$$field_ = 1;
}
);
isolate$inits.push(function(){
  this._nextFreeRefId$$field_ = 0;
}
);
isolate$inits.push(function(){
  isolate$current.double$DartNAN$field = static$uninitialized;
  isolate$current.double$DartINFINITY$field = static$uninitialized;
  isolate$current.double$DartNEGATIVE_INFINITY$field = static$uninitialized;
}
);
isolate$inits.push(function(){
  isolate$current.Duration$DartMILLISECONDS_PER_MINUTE$field = static$uninitialized;
  isolate$current.Duration$DartMILLISECONDS_PER_HOUR$field = static$uninitialized;
  isolate$current.Duration$DartMILLISECONDS_PER_DAY$field = static$uninitialized;
  isolate$current.Duration$DartSECONDS_PER_HOUR$field = static$uninitialized;
  isolate$current.Duration$DartSECONDS_PER_DAY$field = static$uninitialized;
  isolate$current.Duration$DartMINUTES_PER_DAY$field = static$uninitialized;
}
);
RunEntry(unnamedcac5e9$main$member, this.arguments ? (this.arguments.slice ? [].concat(this.arguments.slice()) : this.arguments) : []);
```
