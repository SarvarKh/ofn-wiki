## The Problem

Spree provides a rather flexible but clunky interface for defining variants of a product. The admin can define some system-wide option types (eg. Weight) and option values (eg. "1 kg"), apply the option types to a product, and then select values for these for the variants.

This interface is confusing to learn, and slow and difficult to use, which is a problem when we will have many  enterprise users adopting the system.

Additionally, the way that Spree manages stock control does not always reflect reality. Take the case where we, selling flour, have a bulk store of 1 ton, and we package it into 1 kg and 10 kg bags as we receive orders. Spree provides stock control at the variant level, but then each variant will have its own stock level instead of taking the stock from one central pool for the product.

