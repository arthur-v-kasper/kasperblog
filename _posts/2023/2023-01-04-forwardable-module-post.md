---
layout: post
title: Another Fucking Post
description: Blogs need to have more than one post, so here's another fucking one.
---

{: .large}
Dado o código abaixo:

```ruby
class Saiyan
  attr_accessor :skill
  def initialize
    @skill = [:fly, :ozaru, :super_saiyan]
  end

  def first_skill
    @skill.first
  end
end
goku = Saiyan.new

puts "Fist skill of Goku: #{goku.first_skill}"
```

Funciona certinho e não tem nada de errado, mas com o Forwardable conseguimos fazer isso de uma maneira mais profissional e elegante, saca só:

```ruby
require 'forwardable'

class Saiyan
  attr_accessor :skill

  extend Forwardable

  def_delegator :skill, :first, :first_skill

  def initialize
    @skill = [:fly, :ozaru, :super_saiyan]
  end

end
goku = Saiyan.new

puts "Fist skill of Goku: #{goku.first_skill}"
```

E funciona da mesma forma, neste caso eu adiciono o require do modulo, extendo ele e através do def_delegator, eu digo qual a instância do objeto (@skill), método que quero acessar (first) e nome que estou dando a ele para invoca-lo (first_skill), muito mais lean né?

Tambem é possível delegar mais de um método do objeto usando o `def_delegators`, a diferença é que ele neste caso vc não pode dar nomes, ele assume que o nome é o próprio objeto que vc informou, exemplo

```ruby
require 'forwardable'

class Saiyan
  attr_accessor :skill

  extend Forwardable

  def_delegator :skill, :first, :first_skill
  def_delegators :skill, :last, :size
  def initialize
    @skill = [:fly, :ozaru, :super_saiyan]
  end

end
goku = Saiyan.new

puts "Fist skill of Goku: #{goku.first_skill}"
puts "Last skill of Goku: #{goku.last}"
puts "Size skill of Goku: #{goku.size}"
```

Bacana né? Sem isso eu teria que fazer `goku.skill.last` e `goku.skill.last`

Ou seja, o Forwardable dá a possibilidade de delegar metodos de uma instância para não necessáriamente criar metodos para acessar determinadas propriedades e deixar o código mais enxuto e elegante

[Link doc oficial](https://ruby-doc.org/stdlib-2.5.1/libdoc/forwardable/rdoc/Forwardable.html)
