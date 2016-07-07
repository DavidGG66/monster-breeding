# Monster Breeding

In this program, a "monster" is a (simulated) organism which arises from a
genome. Each genome comes in pairs, inherited from two parents. The goal
of the game is to breed monsters who excel at the game, or who match the
ideals of various breeds at a monster show.

## The Genome

The genome is made up of a number of chromosome pairs, one chromosome from
each parent, where each chromosome is a map from gene ids to allele ids:

[
 [
  {:HGT1 :+
   :HGT2 :-
   :HGT3 :+}
  {:HGT1 :+
   :HGT2 :+
   :HGT3 :-}
 ]
]

This genome has one pair of chromosomes. Each chromosome in the pair has
the same set of genes, :HGT1, HGT2 and :HGT3. Each of these genes has a :+
allele and a :- allele.

The genetic schema stores metadata about each gene; in particular, a
function from two alleles to a compound allele. For instance, we might
have the following:

{:HGT1 {:compound-fn #(if (or (= %1 :+) (= %2 :+)) :+ :-)}}

This compound function defines the :+ allele for :HGT1 to be dominant
and the :- allele to be recessive. That is, if the allele for :HGT1 in
either chromosome of the pair is :+, the compound allele value is also :+.
They both have to be :- in order for the compound value to be :-.

## The Expressor

There is a map which determines the phenotype of an organism based on its
genotype. The observable characteristic :Height would be determined by
this entry in the expressor table:
 
{:Height #(+ (if (= (:HGT1 %) :+) 2 0)
             (if (= (:HGT2 %) :+) 2 0)
             (if (= (:HGT3 %) :+) 1 0))}

This means that the organism's height can vary between 0 and 5, and each
HGT gene independently contributes a weighted amount to that score. This
function is fairly straightforward - things can get arbitrarily complex.
